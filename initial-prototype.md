---
layout: default
title: Initial Prototype
---

<!-- Initial Prototype Section -->
  <section id="about" class="container py-5">
    <h2 class="text-primary">Initial Prototype</h2>
    <p>
      This section goes over the various checkpoints and updates as the initial prototype is being designed. 
    </p>
    <h2 class="text-secondary">Checkpoint 2/19/2026</h2>
    <p>
      Today, readings from the humidity sensor were able to printed to the serial output. This is done using the I2C connection between the humidity sensor (Adafruit     SHT31d) and the microcontroller (Adafruit Trinket M0). 
    </p>
    <h3 class="text-secondary">I2C Connections and Wiring</h3> 
    <p>
      I2C connections are an electrical connection method that uses bidirectional information flow for the sensor and the processor. The two wires are the serial data line (SDA) and the serial clock line (SCL), which are labelled on the SHT31d. In an I2C connection, one device is the "master" (here, the Trinket M0) and the other device is the "slave" (SHT31d), where the master sends requests for data or action, while the slave only responds or operates when ordered to do so. The SHT31D is an I2C type sensor and has SCL and SDA pins, and the Trinket M0 is also I2C compatible. The only SCL compatible pin on the Trinket M0 is D2/A1 (labelled 2 on the Trinket) and the SDA compatible pin is D0/A2 (labelled 0 on the Trinket). This leads to the below connections: 

| Adafruit Trinket Pin | Adafruit SHT31d 2 |
|----------------------|-------------------|
| Trinket M0 Pin 0 | SHT31 SDA | 
| Trinket M0 Pin 2 | SHT31 SCL | 
| Trinket M0 Gnd | SHT31 GND | 
| Trinket M0 3V | SHT31 Vin | 

<img width="1024" height="369" alt="image" src="https://github.com/user-attachments/assets/bcfa3530-957d-40df-a984-a5bf0084884c" />

  </p>
    <h3 class="text-secondary">Trinket Libraries</h3> <p>
      The Adafruit Trinket M0 requires specific libraries to be used with the SHT31d. Four libraries must be imported first: board, time, adafruit_sht31d, and busio.       
      The board library is what maps the physical pins to their digital identifiers (i.e. board.D0, board.D1, etc) which is necessary to communicate which pins are connected to which inputs to print results and manipulate them.      
      The time library is necessary for I2C connections as they permit connection with the SCL line. 
      Similarly, the busio library allows connection with the SDA line. busio needs to be added explicitly to the libraries available on the Trinket M0 as the functions from busio are not built in; this is done by using Adafruit's Github repository of libraries. The folder adafruit_bus_device must be added to the lib folder on the CIRCUITPY(:D) drive that opens when the Trinket M0 is connected to a laptop. 
      The adafruit_sht31d library is the library necessary to interface with the humidity sensor. Similar to busio, the adafruit_sht31d.mpy and adafruit_sht4x.mpy files must be added to the lib folder of the Trinket M0.
    </p>

  <h3 class="text-secondary">Coding the Trinket M0 and Getting Results</h3> <p>
    Coding the controller is done using Mu, which uses Circuit Python (an abridged version of Python) to control the Trinket M0. The code below prints the humidty and temperature of the environment around the SHT31d. 
  </p>

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
  <h2 class="text-secondary">Checkpoint 3/5/2026</h2> 
  <p>
    The goal of this checkpoint was to drive the airpumps (each 4.5V) using a reading from the humidity sensor, but given that the maximum voltage the Trinket M0 can handle is 3V, it was difficult to turn the pumps on in any meaningful way. Each using a 2N222 transistor, the controller could not switch the air pumps on and off without the transistor heating up significantly. Instead, we chose to use an LED as an analog to the airpumps, hoping to use PWM to modulate the LED's output from dim to very bright depending on the reading of the SHT31d. In this checkpoint, we got the PWM to work with the LED based off a reading from the humidity sensor. In this setup, based off the reading from the sensor, the LED (red) switches to BRIGHT or DIM, corresponding to different duty cycle (BRIGHT = 65535, the max val, DIM = 3768 (arbitrary small value)). 
  </p>
  <h3 class="text-secondary">Connections</h3> <p>
    Connection is as follows:

The positive of the battery terminal (4 1.5V batteries (AA) in series) is connected to the collector of a 2N2222 transistor. 

The base of the transistor is connected to pin D4 on the Adafruit Trinket M0 (this is just labelled pin 4). This is how the controller is turning the "switch" (i.e. the transistor) to get different readings. 

The emitter of the transistor is connected to a 220 ohm resistor (red-red-brown-gold, nonprecision) and the resistor is connected to the cathode (long end) of the red LED. I.e. the flow from the emitter is emitter --> 220 ohm resistor --> cathode of LED. The anode of the LED (short end) is connected to the negative terminal of the battery. This is also shared with the ground on the collector. The way this looks is that in one column, the anode, a wire from Gnd on the controller, and the black wire from the batteries are connected. 

The humidity sensor is then connected to the controller. There the Vin on the sensor is connected to 3V on the controller, GND on the controller is connected to GND on the sensor, SCL on the sensor is connected to pin 2 (D2) on the controller, and SDA on the sensor is connected to pin 0 (D0) on the controller. It is important that pin 2 and 0 are used for the sensor since this is an I2C connection, and PWM is available for many of the D pins on the controller but I2C isn't. Therefore pins 0 and 2 should be "saved" for the sensor. 

This circuit is then connected to a laptop via USB-A to micro-USB (like before) and the controller has the following code. The 36.0 threshold was ENTIRELY arbitrary and used for the sake of testing to make sure the LED switched from BRIGHT to DIM. Pick whatever threshold you want. 
  </p>
  <h3 class="text-secondary">Connections</h3> <p>
The following import statements must be included:   </p>

```python
import board
import pwmio
import time
import busio
import adafruit_sht31d
```


