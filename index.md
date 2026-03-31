<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>CBE3300B Project Hub</title>

  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/js/bootstrap.bundle.min.js"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>

<body style="background-color: #f5f5f5; color: #2c3e50;">
  <!-- Header -->
  <header class="text-center py-5" style="background-color: #1e6f5c; color: white;">
    <h1>CBE3300B Project Hub</h1>
    <p>Explore our ongoing and completed project to create a humidity chamber.</p>
    <nav>
      <a href="#about" class="btn btn-outline-light m-2">About</a>
      <a href="#preliminary design report" class="btn btn-outline-light m-2">Preliminary Design Report</a>
      <a href="#initial design report" class="btn btn-outline-light m-2">Initial Design Report</a>
      <a href="#initial prototype" class="btn btn-outline-light m-2">Initial Prototype</a>
      <a href="#gantt" class="btn btn-outline-light m-2">GANTT Schedule</a>
    </nav>
  </header>

  <!-- About Section -->
  <section id="about" class="container py-5">
    <h2 class="text-primary">About</h2>
    <p>
      This repository serves as a hub for our CBE3300B humidity chamber project. The goal is to document, share, and collaborate on research, experiments, and results. 
      Each project section includes its objectives, materials, and development status.
    </p>

  <!-- PDR Section -->
  <section id="preliminary design report" class="container py-5">
    <h2 class="text-primary">Preliminary Design Report</h2>
    <h2 class="text-secondary">Project</h2>
    <p>
    
Humidity chambers are specialized, controlled environments in which a specific relative humidity (RH) value is set. These devices can be used for product quality       assurance, testing material durability, and evaluating shelf-life by simulating specific environments to test aging, moisture ingress, or environmental stress. It can also be used in agriculture for grow-chambers and seed germination. 

In this project the humidity of a chamber is controlled to various set points. Using an air pump, air is run through a flask of water and a flask with dessicator beads. As air is bubbled through the water, the outlet stream of the flask delivers humid air to the chamber. As needed, air is run through the flask with dessicator beads to deliver dry air. Based on a set point, a sensor reads the humidity within the chamber, with the signal sent to a controller that adjusts the flow of humid air and dry air coming from the air pump to reach a desired set point. The PFD of the design is shown below. 

<img width="958" height="674" alt="image" src="https://github.com/user-attachments/assets/4c2dd152-7333-41a0-9a31-7b6c79f1536a" />

<h2 class="text-muted">Physical/Chemical Principles:</h2> 

In this device, a controller is used to change the flow rates of two air streams to manage the relative humidity (RH) of a chamber. Relative humidity is the ratio of the current humidity to the maximum humidity level air can hold at a specific temperature and pressure. This device will run at atmospheric pressure and the chamber will likely be at around room temperature, due to likely negligible heat being transferred from the humid air stream to the chamber.

The RH of the chamber is controlled by air streams from two air pumps. Two streams are mixed and delivered to the chamber: a dry air stream with negligible water vapor content and a humidified air stream formed by bubbling air through water. By adjusting the relative flow rates of these streams, the ratio of humid air to dry air increases or decreases, which increases or decreases the RH of the chamber. The water is being kept at a constant temperature to ensure that the RH of the humid air input stream remains constant; in this way, only adjustment of the flow rate changes the RH of the chamber. Another level of RH control of the chamber can come from changing the temperature of the water. If the water's temperature increases, the humidity of the input humid stream also increases, making drastic increases to the RH of the chamber possible.

There are no reactions in this device. There are no corrosive materials either. 

<h2 class="text-muted">Calculations:</h2> 

There are no calculations necessary. We will be using a PID algorithm and the humidity sensor automatically calculates the RH. 

<h2 class="text-muted">Controller Summary:</h2> 

The Arduino acts as the central controller for the system and provides power and control signals to the circuit. All components share a common ground so they can communicate properly. A humidity sensor inside the chamber continuously measures the relative humidity and sends this information to the Arduino. The desired humidity level is set using a potentiometer, which the Arduino reads as an input. Since the air pumps require more power than the Arduino can supply directly, they are controlled using transistor switch circuits that allow the Arduino to safely turn the pumps on and off. Diodes are included across the pumps to protect the circuit when the pumps are switched off. An LED is used as a simple visual indicator to show when the system is operating. Together, these components allow the system to monitor and adjust humidity in the chamber in a controlled and reliable way.

The control error is defined as the difference between the desired humidity setpoint and the measured chamber humidity. The PID controller computes a control signal based on the instantaneous error, the accumulated error over time, and the rate of change of the error. This combined response enables fast correction of deviations while minimizing steady-state offset and oscillations. The control unit we will be using is the Adafruit control unit (Adafruit Trinket M0) which can be controlled using Python. The microcontroller here will be using pulse width modulation, or PWM, to control the air pumps. PWM allows for the air pumps to be rapidly turned on and off, which allows for close control of the humid and dry air streams, preventing large overshoot. Using a PID algorithm also helps prevent over/undershoot. 

The input to the controller is the reading from the humidity sensor (Adafruit Sensirion SHT31-D). This humidity controller directly measures the RH of the chamber and can be connected to the Trinket M0 using the pin-out from the manual. Thus the flow of the electrical signal goes from humidity controller to Adafruit microcontroller to air pump(s). A nondigital, manual input can also be added using a potentiometer, where the voltage can be varied to vary the desired set point of the microcontroller. The potentiometer can be connected to an analog input on the Trinket M0, which can then be read in the code to vary the set point. 

<h2 class="text-muted">Circuit Principles:</h2> 

The Arduino 5 V pin is connected to the power rail of the breadboard, and the Arduino GND pin is connected to the ground rail. The ground of the external pump power supply is also connected to the same ground rail to ensure a common electrical reference.

The humidity sensor is placed inside the chamber and connected as follows to the breadboard:
Sensor VIN → breadboard 5 V rail
Sensor GND → breadboard ground rail
Sensor SDA → Arduino SDA pin
Sensor SCL → Arduino SCL pin

A 10 kΩ potentiometer is placed across the breadboard gap:
- One outer leg → 5 V rail
- Other outer leg → ground rail
- Middle leg → Arduino analog input A0

Each air pump is controlled using a transistor switch circuit assembled on the breadboard.
- Arduino digital pin (e.g., D8 or D9) → 220 Ω resistor
- Other side of resistor → base of PN2222 transistor
- Emitter of transistor → ground rail
- Collector of transistor → negative terminal of pump
- Positive terminal of pump → external power supply positive

A diode is connected across the pump:
- Diode cathode (striped end) → pump positive terminal
- Diode anode → transistor collector

An LED is placed on the breadboard:
- Arduino digital pin (e.g., D7) → 220 Ω resistor
- Resistor → LED long leg (anode)
- LED short leg (cathode) → ground rail

<h2 class="text-muted">Item list</h2> 

<table>
  <thead>
    <tr>
      <th>Item</th>
      <th>Quantity</th>
      <th>Link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Chamber</td>
      <td>1</td>
      <td><a href="https://tinyurl.com/othercontainer" target="_blank">Amazon Suclain 2 Pcs Square Food Storage Container</a></td>
    </tr>
    <tr>
      <td>Plastic Resevoirs (humid and dry airlines)</td>
      <td>2</td>
      <td><a href="https://www.amazon.com/ASEVAT-Leak-proof-Container-Translucent-Containers/dp/B0DSP2WHTW/" target="_blank">ASEVAT 10 Pcs 2 oz Airtight Pill Bottles Wide Mouth Reagent Lab Bottle</a></td>
    </tr>
    <tr>
      <td>Dessicator beads</td>
      <td>0.5 lbs</td>
      <td><a href="https://www.amazon.com/Premium-Indicating-Silica-Industry-Standard/dp/B013L31PQ0/" target="_blank">Silica Desiccants Beads</a></td>
    </tr>
    <tr>
      <td>3 mm ID Silicon Tubing</td>
      <td>2 rolls</td>
      <td><a href="https://www.adafruit.com/product/4661" target="_blank">Silicone Tubing for Air Pumps and Valves</a></td>
    </tr>
    <tr>
      <td>Air pumps</td>
      <td>2</td>
      <td><a href="https://www.adafruit.com/product/4699" target="_blank">Adafruit Air Pump and Vacuum DC Motor - 4.5 V and 2.5 LPM</a></td>
    </tr>
    <tr>
      <td>Microcontroller</td>
      <td>1</td>
      <td><a href="https://www.adafruit.com/product/3500" target="_blank">Adafruit Trinket M0</a></td>
    </tr>
    <tr>
      <td>Humidity Sensor</td>
      <td>1</td>
      <td><a href="https://www.adafruit.com/product/2857" target="_blank">Adafruit SHT31 Humidity Sensor</a></td>
    </tr>
    <tr>
      <td>Potentiometer</td>
      <td>1</td>
      <td><a href="https://www.adafruit.com/product/4272" target="_blank">10 kΩ potentiometer</a></td>
    </tr>
    <tr>
      <td>Breadboard</td>
      <td>2</td>
      <td><a href="https://www.adafruit.com/product/1609" target="_blank">Adafruit Breadboard</a></td>
    </tr>
    <tr>
      <td>Heating Pad</td>
      <td>2</td>
      <td><a href="https://www.adafruit.com/product/1481" target="_blank">Electric Heating Pad - 10cm x 5cm</a></td>
    </tr>
  </tbody>
</table>

  </p>
  </section>

   <!-- Initial Design Report Section -->
  <section id="initial design report" class="container py-5">
    <h2 class="text-primary">Initial Design Report</h2>
    <p>
      
<h2 class="text-muted">Product Concept:</h2> 
ClimaPod is a compact, low-cost humidity-controlled chamber designed to provide precise environmental control within small, enclosed volumes. In many laboratory and small-scale storage settings, humidity can fluctuate significantly due to ambient conditions, making it difficult to maintain stable moisture levels using passive methods alone. ClimaPod addresses this challenge by enabling users to specify target humidity set points and actively regulating the internal environment to maintain those conditions over time.

![ClimaPod](https://raw.githubusercontent.com/rithikar1510/CBE-3300B/main/ClimaPod.jpg)

<h2 class="text-muted">Design Details:</h2> 
This diagram illustrates the overall design concept of ClimaPod and how humidity is actively regulated within the chamber. Two separate air streams are generated using individual air pumps. One air stream is passed through a container filled with desiccant beads, which removes moisture from the air and produces a dry air stream. The second air stream is bubbled through heated water, allowing the air to become saturated with moisture before entering the chamber as humidified air. These two streams are introduced into the chamber, where they mix to produce the desired humidity level.

A humidity sensor placed inside the chamber continuously monitors the internal environmental conditions and sends this information to the controller. Based on the difference between the measured humidity and the desired set point, the controller adjusts the relative flow rates of the dry and humid air streams. This closed-loop control strategy allows ClimaPod to maintain stable humidity over time, even in the presence of disturbances such as changes in ambient conditions or small leaks in the chamber.

![ClimaPod Design Concept](https://raw.githubusercontent.com/rithikar1510/CBE-3300B/main/Design_concept.jpg)

<h2 class="text-muted">Market Research:</h2> 

Humidity chambers have a variety of different uses: plant growth, product testing, as a humidor, and for art or rare material preservation. Also known as climatic test chambers, products are placed inside the chamber and subjected to temperature and relative humidity cycles to determine their resistance. Different products include electronics, circuit boards, automotive components, pharmaceuticals, moisture-resistant materials, and consumer goods intended to have long-shelf lives. These chambers are important for product testing since they enable producers to simulate real-world situations. For example, circuit boards and electronics must be corrosion resistant to ensure longevity and efficiency, and moisture-resistant packaging and consumer goods must be tested for shelf life for FDA restrictions. 

The global market for climatic test chambers is experiencing strong growth, with the 2024 valuation of the market being around 2.4 billion USD, with a CAGR of 2% CAGR to exceed 2.4 billion USD by 2032. Demand is growth due to strong product reliability and safety regulations and expectations from consumers and governments. Furthermore, growth of consumer electronics has pushed the demand for product validation, with especially strict standards for the aerospace and defense industries, who use these test chambers to test parts for their efficacy even in varying climatic conditions. 

Furthermore, integration of automation and artificial intelligence is pushing the abilities of these test chambers, where remote interfacing and real-time monitoring is pushing the abilities of these chambers. These chambers are thousands of dollars, with the smallest models starting at $5,000 for a 34 L system. However, these chambers also tend to couple temperature control, and they can go from -70 C to 183 C. Even the smallest models begin at $1000. 

<h2 class="text-muted">Market Niche</h2> 

In the market, there is a gap for very small, budget climatic test chambers, especially chambers that only control humidity if temperature is not a variable of concern. The largest cost driver is the temperature control system, as the refridgeration system and heating elements are very expensive due to the number and the quality of the compressors and electronics required. Futhermore, the humidity sensors used for industry R&D have low ranges and margins of error in measurement, leading to a costlier subsystem. These two subsystems together are around 65% of the cost. The rest of the cost comes from the interface, control electronics, and the insulating material, which the most effective chambers used higher cost insulating materials. Our chamber hopes to occupy a space for a cheaper humidity chamber that only focuses on humidity, eliminating one subsystem. The total cost of our chamber would be $68.50, which is far below commercial options. This device would satisfy small scale product testing, such as for academic research or for home-use for preservation of plants, family heirlooms/delicate items, or DIY product development. 

<h2 class="text-muted">Price Breakdown:</h2> 

<h2 class="text-muted">SWOT Analysis:</h2> 

<img width="1118" height="628" alt="image" src="https://github.com/user-attachments/assets/d982f4b4-1707-4d95-b286-eda12781a12c" />

</p>

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
|---|---|---|
| Trinket M0 Pin 0 | SHT31 SDA | 
| Trinket M0 Pin 2 | SHT31 SCL | 
| Trinket M0 Gnd | SHT31 GND | 
| Trinket M0 3V | SHT31 Vin | 

<img width="1024" height="369" alt="image" src="https://github.com/user-attachments/assets/bcfa3530-957d-40df-a984-a5bf0084884c" />

<img width="1024" height="779" alt="image" src="https://github.com/user-attachments/assets/90e3d529-8db6-499e-bd77-20dbe5156854" />

  </p>
    <h3 class="text-secondary">Trinket Libraries</h3> <p>
      The Adafruit Trinket M0 requires specific libraries to be used with the SHT31d. Four libraries must be imported first: board, time, adafruit_sht31d, and busio. The board library is what maps the physical pins to their digital identifiers (i.e. board.D0, board.D1, etc) which is necessary to communicate which pins are connected to which inputs to print results and manipulate them. The time library is necessary for I2C connections as they permit connection with the SCL line. Similarly, the busio library allows connection with the SDA line. 
    </p>


  <!-- GANTT Schedule Section -->
  <section id="gantt" class="container py-5">
    <h2 class="text-primary">Project Timeline</h2>
    <p>View our project schedule using the GANTT chart:</p>
    <p>
      <a href="https://docs.google.com/spreadsheets/d/12-z6ueIstVCXI8U_jozlMdH24LCAA0FeEc4j1kiNmzk/edit?usp=sharing" target="_blank" class="btn btn-success">
        📅 View GANTT Schedule
      </a>
    </p>
    <iframe src="https://docs.google.com/spreadsheets/d/12-z6ueIstVCXI8U_jozlMdH24LCAA0FeEc4j1kiNmzk/edit?usp=sharing" width="100%" height="600px" style="border: none;"></iframe>
  
</body>
</html>
