---
layout: default
title: Intermediate Prototype
---

# Minimum Viable Product

## Checkpoint 4/16/2026

### Getting Started with Feather RP2040

Today we connected the Adafruit Feather RP2040 to the various pieces of the set-up to see if it works. To set up the Adafruit Feather, you can't simply attach via USB to a laptop. Instead you have to open the bootloader; this can be done by plugging in the Feather (USB-A to USB-C, data transfer cable) to the laptop, then holding the bootloader and reset button at the same time. 

<img width="1024" height="498" alt="image" src="https://github.com/user-attachments/assets/fb74e6c8-76e3-4088-85a8-61ba49dc4238" />

When the buttons are held at the same time, a drive called RPI-RP2 will open on your laptop. Then, you have to add the CircuitPython file (downloadable here: https://learn.adafruit.com/adafruit-feather-rp2040-pico/circuitpython) to the RPI-RP2 drive. The RPI-RP2 drive is the bootloader; adding in the CircuitPython file means that the CIRCUITPY drive will open, meaning you can use Mu (as done for the Trinket M0) to code the Feather with CircuitPython. Once the file is added, CIRCUITPYTHON will automatically open. Then open the code.py file in Mu like before. 

The goal of this day was to get the display (Adafruit Monochrome 1.12" 128x128 OLED Graphic Display) to display the set point for the humidity and the humidity reading from the SHT31d. The OLED display also uses an I2C connection. Both the OLED display and the SHT31d can be connected via the same I2C bus; the default address (in our connection) was the OLED at 0xD and the SHT3d at 0x44, but the alternative addresses are 0xC and 0x45. 

To use the OLED display, the adafruit_bitmap_font and adafruit_ssd1306 libraries have to be imported (download libraries from here: https://circuitpython.org/libraries). In addition, the font5x8.bin file (download here: https://github.com/adafruit/Adafruit_CircuitPython_framebuf/blob/main/examples/font5x8.bin) has to be added to the CIRCUITPY drive; this isn't added to the lib file like other libraries--just add it to the root of the drive. Once all the relevant files are downloaded, connect the SHT31d and the OLED to the same buses (Clk on OLED and SCL of both SHT31d and Feather to common pins, Data on the OLED and SDA of SHT31d and Feather to common pins, common Vin/3.3V pin and common GRD). Make sure you know which addresses the OLED and the sensor are connected to-the following code tells you which addresses the I2C connections are on. The format should look like ['0x44', '0x3D'].

```python
import board
import busio
import time

i2c = busio.I2C(board.SCL, board.SDA)

while not i2c.try_lock():
    pass

print("I2C addresses found:")
print([hex(x) for x in i2c.scan()])

i2c.unlock()

while True:
    time.sleep(1)
```

### Using SSD1306

To use I2C to read off the OLED, you have to create an instance of the SSD1306 class and initialize the I2C firmware using the busio library. First, we created an I2C setup with i2c = busio.I2C(board.SCL, board.SDA). Then, we created an OLED object using adafruit_ssd1306.SSD1306_I2C(). This function takes in the widtch of the display, the heigh, the type of connection, and the address that the object can be found at. For the OLED we used, the line was: adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3d). Once you know the addresses, the code takes the reading of the SHT31d and prints it on the OLED every 2 seconds:

```python
import time
import board
import busio
import adafruit_ssd1306
import adafruit_sht31d

# I2C setup
i2c = busio.I2C(board.SCL, board.SDA)

# Sensor
sht31 = adafruit_sht31d.SHT31D(i2c)

# OLED (explicit address if needed)
display = adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3D)

while True:
    humidity = sht31.relative_humidity

    display.fill(0)

    # Use show() BEFORE text (important fix for some versions)
    display.show()

    # Draw text AFTER clearing buffer
    display.text("Humidity:", 0, 10, 1)
    display.text("{:.1f}%".format(humidity), 0, 30, 1)

    display.show()

    print("Humidity:", humidity)

    time.sleep(2)
```
We couldn't find any way to make the font size larger. The best way to fix this is likely by getting a bigger OLED display. Once this works, we used the connections of before (MOSFETS) to use PWM to drive the pumps. 

### Potentiometer

The following code shows the humidity and the set point on the display as an RH value. The set point modulation comes from a potentiometer. We had a sliding 10k potentiometer - the back is labelled with pins 1,2, and 3. Pin 1 is GRD, 3 is 3.3V, and pin 2 connects to one of the analog pins on the Feather. Using the analogio library in CircuitPython (automatically installed, does NOT need to be downloaded from the libraries list/added to CIRCUITPY), read the voltage off the potentiometer using AnalogIn(). Using .value, Mu scales the analog value as a digital value in the 16 bit integer range (i.e. min = 0, max = 65535). This can be scaled as a percentage using the formula: RH_SP = 100 * (pot.value/65535). We assumed that the pumps and control logic being used isn't specific enough to keep values EXACTLY on the set point, so we decided an intended range of +/- 2%. This determined the HIGH_THRESHOLD and LOW_THRESHOLD values. 

The goal for the next checkpoint is to solder everything and add some type of switch; an idea could be a button to initialize everything. Else, on/off control can purely come from turning the power source on and off. In this date, we also tried decoupling the setup from the laptop using the battery - it works! 

```python
import time
import board
import busio
import adafruit_ssd1306
import adafruit_sht31d
from analogio import AnalogIn
import pwmio

# I2C setup
i2c = busio.I2C(board.SCL, board.SDA)

#potentiometer for set point adjustment
pot = AnalogIn(board.A3)

# Sensor
sht31 = adafruit_sht31d.SHT31D(i2c)

#humid pump
pump1 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0)  # HUMID

#dry pump
pump2 = pwmio.PWMOut(board.D5, frequency=1000, duty_cycle=0)  # DRY

# OLED (explicit address is 0x3D)
display = adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3D)

OFF = 0
HALF = 32768
QUARTER = 16384
FULL = 65535

while True: 

    #read values off hardware
    SET_POINT = 100 * (pot.value/65535)
    HIGH_THRESHOLD = SET_POINT + 2
    LOW_THRESHOLD = SET_POINT - 2
    humidity = sht31.relative_humidity

    #display 
    display.fill(0)
    display.show()
    display.text("Humidity:", 0, 5, 1)
    display.text("{:.1f}%".format(humidity), 0, 20, 1)
    display.text("Set point:", 0, 35, 1)
    display.text("{:.1f}%".format(SET_POINT), 0, 50, 1)
    display.show()
    
    #control logic
    if humidity is not None:
        if (humidity > HIGH_THRESHOLD):
            pump2.duty_cycle = OFF #turn off dry pump
            if (humidity - HIGH_THRESHOLD) < 2:
                pump1.duty_cycle = HALF
            else:
                pump1.duty_cycle = FULL
        elif humidity < LOW_THRESHOLD:
            pump1.duty_cycle = OFF #turn humid off
            if (LOW_THRESHOLD - humidity) < 2:
                pump2.duty_cycle = QUARTER
            else:
                pump2.duty_cycle = FULL
        else:
            pump1.duty_cycle = OFF
            pump2.duty_cycle = OFF

    time.sleep(0.5)

```

<img width="1179" height="1450" alt="labelled setup" src="https://github.com/user-attachments/assets/3e52680e-e749-40ec-a69c-e30d13eb520c" />

<img width="1179" height="1450" alt="IMG_2885" src="https://github.com/user-attachments/assets/659d74c4-fce9-485b-9209-5fc1778cecb8" />

## Checkpoint 4/23/26

### Power Sourcing and Troubleshooting

After the end of last class, the goal was to decouple from the laptop and decouple everything. Today, we soldered everything on, but everything but the display worked. It would flash on for a moment and then turn off when the pumps were stil turned on. We tried commenting out the code that switched pumps on and off, and left everything else the same, and the entire device worked minus modulation of the pumps. This means the pumps being on was inhibiting the display function, indicating some sort of power sag/source issue, not a control/wiring issue. 

When soldering, we had switched from a battery power source to a plug in adapter that we had cut into. To find the ground vs the power wire in the adapter, we used a multimeter, and then soldered into the common ground/Vin rails of the breadboard respectively. However, we noticed that though the plug was rated for 5V, the max current was only 1 A. The pumps can draw up to 500 mA each, and we assumed that the pumps, at least at the start, were drawing 500 mA and maxing out the current from the adapter and leaving nothing for the potentiometer to work. To fix this, we found a 2A, 5V plug-in power source, which once soldered onto the breadboard worked due to its higher current capacity. All pieces of the device are working! 

### Code

This is the final version of the code. Here, like explained in the last checkpoint, proportional control modulates the speed of each pump to prevent overshoot. The range at which proportional control switches on is +/- 4% RH; if the measured RH is more than 4% away from the set point, the pump simply operations at full power.

```python
import time
import board
import busio
import adafruit_ssd1306
import adafruit_sht31d
from analogio import AnalogIn
import pwmio

# I2C setup
i2c = busio.I2C(board.SCL, board.SDA)

#potentiometer for set point adjustment
pot = AnalogIn(board.A3)

# Sensor
sht31 = adafruit_sht31d.SHT31D(i2c)

#humid pump
pump1 = pwmio.PWMOut(board.D5, frequency=1000, duty_cycle=0)  # HUMID

#dry pump
pump2 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0)  # DRY

# OLED (explicit address iis 0x3D)
display = adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3D)

OFF = 0
MIN_VAL = 17500
FULL = 65535

humidity = sht31.relative_humidity

while True:
    SET_POINT = 100 * (pot.value/65535)
    humidity = sht31.relative_humidity
    HIGH_THRESHOLD = SET_POINT + 0.5
    LOW_THRESHOLD = SET_POINT - 0.5

    display.fill(0)
    display.show()
    display.text("Humidity:", 0, 5, 1)
    
    print("this is set", SET_POINT)
    print(humidity)

    if humidity is not None:
        display.text("{:.1f}%".format(humidity), 0, 20, 1)
    else:
        display.text("Sensor error", 0, 20, 1)

    display.text("Set point:", 0, 35, 1)
    display.text("{:.1f}%".format(SET_POINT), 0, 50, 1)
    display.show()

    # Control logic
    if humidity is not None:
        if humidity > HIGH_THRESHOLD:
            pump2.duty_cycle = OFF
            perc_difference = (humidity - HIGH_THRESHOLD)/4
            power_val = int(perc_difference * FULL)
            if (humidity - HIGH_THRESHOLD) < 4:
                if power_val < MIN_VAL: 
                    pump1.duty_cycle = MIN_VAL
                else: 
                    pump1.duty_cycle = power_val
            else:
                pump1.duty_cycle = FULL
        elif humidity < LOW_THRESHOLD:
            pump1.duty_cycle = OFF
            perc_difference = (LOW_THRESHOLD - humidity)/4
            power_val = int(perc_difference * FULL)
            if (LOW_THRESHOLD - humidity) < 4:
                if power_val < MIN_VAL: 
                    pump2.duty_cycle = MIN_VAL
                else: 
                    pump2.duty_cycle = power_val
            else:
                pump2.duty_cycle = FULL
        else:
            pump1.duty_cycle = OFF
            pump2.duty_cycle = OFF
    time.sleep(1)
```

To find the threshold limit of 4%, we created various calibration curves and attempted to minimize the deviations from the set point. We decided on 4% as the optimal variation, but its possible that further tuning could be carried out here. The calibration curves are shown below. The time needed to reach +/- 4.5% deviation from the set point is around 25 seconds in both directions when initially around 50% from the set point.

<img width="878" height="541" alt="image" src="https://github.com/user-attachments/assets/f0909989-9dad-4517-a77d-75ea2143823c" />

<img width="889" height="542" alt="image" src="https://github.com/user-attachments/assets/27483578-9abd-4526-9a87-b523499443b8" />

### Hardware

We decided to encapsulate all of our parts in a 3D printed box. The box was designed with a lid with holes in for the dry and humid lines, as well as a hole at the bottom through which wiring for the power source and the humidity sensor runs through. We also drilled holes in the top of the box for connections to the display and the potentiometer. This makes the display easily visible and the potentiometer easily accessible to modulate the set point. We also wrapped the air pumps in bubble wrap to reduce noise and also hopefully prevent damage to either the breadboard or causing loose wires/piping. The flasks with the dessicator beads and water were ensured to be very tightly closed and when the box is closed are ensured to be upright to both prevent the beads/water from entering the pipe and from spilling in the box. Pictures of the finished device are included below. 

<img width="530" height="542" alt="image" src="https://github.com/user-attachments/assets/cad0c7b7-e9c5-4d51-90e9-3497bc46ed03" />

<img width="535" height="675" alt="image" src="https://github.com/user-attachments/assets/19893fc4-d056-4f36-b85b-61ccc3972fb3" />

<img width="557" height="737" alt="image" src="https://github.com/user-attachments/assets/9a728ba0-7a0b-49cc-afa4-8cfffe045a32" />

<img width="721" height="868" alt="image" src="https://github.com/user-attachments/assets/a3b65f95-0456-4a3d-9369-a90d8bc072f4" />




