---
layout: default
title: Initial Prototype
---

# Initial Prototype

This section goes over the various checkpoints and updates as the initial prototype is being designed. 

## Checkpoint 2/19/2026

Today, readings from the humidity sensor were able to printed to the serial output. This is done using the I2C connection between the humidity sensor (Adafruit SHT31d) and the microcontroller (Adafruit Trinket M0). 

### I2C Connections and Wiring

I2C connections are an electrical connection method that uses bidirectional information flow for the sensor and the processor. The two wires are the serial data line (SDA) and the serial clock line (SCL), which are labelled on the SHT31d. In an I2C connection, one device is the "master" (here, the Trinket M0) and the other device is the "slave" (SHT31d), where the master sends requests for data or action, while the slave only responds or operates when ordered to do so. The SHT31D is an I2C type sensor and has SCL and SDA pins, and the Trinket M0 is also I2C compatible. The only SCL compatible pin on the Trinket M0 is D2/A1 (labelled 2 on the Trinket) and the SDA compatible pin is D0/A2 (labelled 0 on the Trinket). This leads to the below connections:

| Adafruit Trinket Pin | Adafruit SHT31d 2 |
|----------------------|-------------------|
| Trinket M0 Pin 0     | SHT31 SDA         |
| Trinket M0 Pin 2     | SHT31 SCL         |
| Trinket M0 Gnd       | SHT31 GND         |
| Trinket M0 3V        | SHT31 Vin         |

![image](https://github.com/user-attachments/assets/bcfa3530-957d-40df-a984-a5bf0084884c)

### Trinket Libraries

The Adafruit Trinket M0 requires specific libraries to be used with the SHT31d. Four libraries must be imported first: board, time, adafruit_sht31d, and busio.       

The board library is what maps the physical pins to their digital identifiers (i.e. board.D0, board.D1, etc) which is necessary to communicate which pins are connected to which inputs to print results and manipulate them.      

The time library is necessary for I2C connections as they permit connection with the SCL line. 

Similarly, the busio library allows connection with the SDA line. busio needs to be added explicitly to the libraries available on the Trinket M0 as the functions from busio are not built in; this is done by using Adafruit's Github repository of libraries. The folder adafruit_bus_device must be added to the lib folder on the CIRCUITPY(:D) drive that opens when the Trinket M0 is connected to a laptop. 

The adafruit_sht31d library is the library necessary to interface with the humidity sensor. Similar to busio, the adafruit_sht31d.mpy and adafruit_sht4x.mpy files must be added to the lib folder of the Trinket M0.

### Coding the Trinket M0 and Getting Results

Coding the controller is done using Mu, which uses Circuit Python (an abridged version of Python) to control the Trinket M0. The code below prints the humidty and temperature of the environment around the SHT31d. 

```python
import board
import busio
import adafruit_sht31d
import time

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

while True:
    print("Temp:", sensor.temperature)
    print("Humidity:", sensor.relative_humidity)
    print("-----")
    time.sleep(2)
```
## Checkpoint 3/5/2026

The goal of this checkpoint was to drive the airpumps (each 4.5V) using a reading from the humidity sensor, but given that the maximum voltage the Trinket M0 can handle is 3V, it was difficult to turn the pumps on in any meaningful way. Each using a 2N222 transistor, the controller could not switch the air pumps on and off without the transistor heating up significantly. Instead, we chose to use an LED as an analog to the airpumps, hoping to use PWM to modulate the LED's output from dim to very bright depending on the reading of the SHT31d. In this checkpoint, we got the PWM to work with the LED based off a reading from the humidity sensor. In this setup, based off the reading from the sensor, the LED (red) switches to BRIGHT or DIM, corresponding to different duty cycle (BRIGHT = 65535, the max val, DIM = 3768 (arbitrary small value)).

### Connections

Connection is as follows:

The positive of the battery terminal (4 1.5V batteries (AA) in series) is connected to the collector of a 2N2222 transistor.

The base of the transistor is connected to pin D4 on the Adafruit Trinket M0 (this is just labelled pin 4). This is how the controller is turning the "switch" (i.e. the transistor) to get different readings.

The emitter of the transistor is connected to a 220 ohm resistor (red-red-brown-gold, nonprecision) and the resistor is connected to the cathode (long end) of the red LED. I.e. the flow from the emitter is emitter --> 220 ohm resistor --> cathode of LED. The anode of the LED (short end) is connected to the negative terminal of the battery. This is also shared with the ground on the collector. The way this looks is that in one column, the anode, a wire from Gnd on the controller, and the black wire from the batteries are connected.

The humidity sensor is then connected to the controller. There the Vin on the sensor is connected to 3V on the controller, GND on the controller is connected to GND on the sensor, SCL on the sensor is connected to pin 2 (D2) on the controller, and SDA on the sensor is connected to pin 0 (D0) on the controller. It is important that pin 2 and 0 are used for the sensor since this is an I2C connection, and PWM is available for many of the D pins on the controller but I2C isn't. Therefore pins 0 and 2 should be "saved" for the sensor.

This circuit is then connected to a laptop via USB-A to micro-USB (like before) and the controller has the following code. The 36.0 threshold was ENTIRELY arbitrary and used for the sake of testing to make sure the LED switched from BRIGHT to DIM. Pick whatever threshold you want.

### Code

```python
import board
import pwmio
import time
import busio
import adafruit_sht31d

led = pwmio.PWMOut(board.D4, frequency=5000, duty_cycle=0)

i2c = busio.I2C(board.D2, board.D0)
sht = adafruit_sht31d.SHT31D(i2c)

print("Starting humidity/LED test")

threshold = 36.0

BRIGHT = 65535
DIM = 3768

while True:
    hum = sht.relative_humidity
    temp = sht.temperature
    print(f"Temp: {temp:.1f} C  Humidity: {hum:.1f}%")

    if hum > threshold:
        led.duty_cycle = DIM
        print("LED DIM")
    else:
        led.duty_cycle = BRIGHT
        print("LED BRIGHT")
    
    time.sleep(2)
```
## Checkpoint 3/24/26

Due to the lower voltage from the Trinket M0, 2 MOSFETs (Adafruit MOSFET Driver - For Motors, Solenoids, LEDs, etc - STEMMA JST PH 2mm) were purchased. Two MOSFET's are necessary because there are two pumps, and each pump requires its own MOSFET so that the lower voltage from the Trinket M0 can be used to modulate these higher voltage motors. In tihs setup, a common power source (4 1.5V batteries - i.e. 4 AA batteries) were used to power the MOSFETs. GRD was supplied by the Trinket M0 (GRD pin) and it is important that the negative terminal of the batteries, GRD of the MOSFETS, and GRD of the SHT31d are all common grounds. This was just on-off control; no PWM was used yet.

### Preliminary Motor Testing 

This was just on-off control; no PWM was used yet.

#### Connections
Connect the battery to the MOSFETS (MOSFETS have a + and - labelled for the motor wires. Plug in + of battery to + of MOSFET and - of battery to - of MOSFET. One battery (4 1.5 V batteries) is enough for both MOSFETS. Then connect GRD of MOSFET (using 3 wires or the pins) to common GRD with Trinket M0 and SHT31d, Vin to + battery, and the In pin/wire to D3 and D4 (3 and 4) on the Trinket M0. For preliminary test of motors, where they'll alternate for 2 sec apiece:

```python
import board
import digitalio
import time

motor1 = digitalio.DigitalInOut(board.D3)
motor1.direction = digitalio.Direction.OUTPUT

motor2 = digitalio.DigitalInOut(board.D4)
motor2.direction = digitalio.Direction.OUTPUT

while True:
    print("Motor 1 ON")
    motor1.value = True
    motor2.value = False
    time.sleep(3)
    print("Motor 2 ON")
    motor1.value = False
    motor2.value = True
    time.sleep(3)
```

### Air Pumps with SHT31d Testing

Here, after checking that the air pumps could be driven, do the usual connection of SHT31d to the Trinket M0 (see above for wiring). Run the following code. As humidity is above 40, one motor turns on. As humidity is below 20, the other motor turns on. These thresholds were arbitrary and chosen to reflect the idea that a range of values could be set as the set point (with range being the range of uncertainty), and the air pumps turn off and on to achieve this range. Here, it could be said that the set point is 30% RH, with an uncertainty of +/- of 10%.

#### Code

```python
import board
import busio
import digitalio
import time
import adafruit_sht31d

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

pump_high = digitalio.DigitalInOut(board.D3)
pump_low = digitalio.DigitalInOut(board.D4)

pump_high.direction = digitalio.Direction.OUTPUT
pump_low.direction = digitalio.Direction.OUTPUT

HIGH_THRESHOLD = 40
LOW_THRESHOLD = 20

while True:
    humidity = sensor.relative_humidity
    print("Humidity:", humidity)

    if humidity is not None:
        if humidity > HIGH_THRESHOLD:
            pump_high.value = True
            pump_low.value = False
        elif humidity < LOW_THRESHOLD:
            pump_high.value = False
            pump_low.value = True
        else:
            pump_high.value = False
            pump_low.value = False

    time.sleep(2)
```
## Initial Prototype Demo 3/26/2026

Prototype was demonstrated well! airpumps were connected to the appropriate tubing (whichever air pump turned on when RH > HIGH_THRESHOLD was connected to the tube with desicator beads and the other to a container with water at room temperature). Air pumps modulated correctly, but there was splatter through the tubing connected to the water-filled tube.

Next steps:

- Move tubing such that splatter can be avoided. Maybe connected the tube to the top of the tube instead of the side will prevent liquid from entering the tube. Mesh could also be added between the liquid level and the tubing's entrance to prevent splatter.
- Connections should be soldered, with thought to where components will be. The two MOSFETs should be on the same breadboard, with the humidity sensor attached via the four wires (GRD, Vin, SCL, SDA) through small holes in the container. The Trinket M0 should be attached to the same breadboard as the MOSFETs. This is so that there can be a common ground.
- PWM should be tried for more precise control (or PID, etc)
