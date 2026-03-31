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
