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



| Item | Quantity | Link |
| --- | --- | --- |
| Chamber | 1 | <a href="https://tinyurl.com/othercontainer">Amazon Suclain 2 Pcs Square Food Storage Container</a> |
| Plastic Resevoirs (humid and dry airlines) | 2 | <a href="https://www.amazon.com/ASEVAT-Leak-proof-Container-Translucent-Containers/dp/B0DSP2WHTW/ref=sr_1_10_sspa?crid=3GUXLGM9JGLI9&dib=eyJ2IjoiMSJ9.JCFmWnBryUBxMzInTekbPmtgNwl72-cKg6FuGYImiB-_aKz8iKfssCPKzVd3gqb_Id1VPtukAdbMhwt8qb_57fkYIyeJ9PkAphm6OQOa_R5fiMmKDQq_nrT8Dpqq61jdk-cIqxgM1HRBleR3eDLIm3BTtYHzjv9kPdqfda1g9jzy_bexMM_D-DdhXdr6Eh1njzrBNQBtVh9Vm-iZRjCWCr5PFz1vT93xT3GFC0JxWjuA-_VgsBnZEcZY-b_FKJcPx4OpF3kbGe8fCYF7ZyXTi_zT4EyjWfuSzanB_6mmRYo.EcwL_ghv7Aq6NKJX0SNyQh-HNkv2rkxLvBeKLHkOi5o&dib_tag=se&keywords=plastic%2Bvials&qid=1769724905&s=industrial&sprefix=plastic%2Bvials%2B%2Cindustrial%2C105&sr=1-10-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9tdGY&th=1">ASEVAT 10 Pcs 2 oz Airtight Pill Bottles Wide Mouth Reagent Lab Bottle</a> |
| Dessicator beads | 0.5 lbs | <a href="https://www.amazon.com/Premium-Indicating-Silica-Industry-Standard/dp/B013L31PQ0/ref=sr_1_3?crid=RVISUUHO3UJ0&dib=eyJ2IjoiMSJ9._DjEw0TeT2_ClWLr2xpFAjEGwNT1-fvj-2YUOqST7cjMp37gEEVuNHrUyKnISaTJPYOsKh0TMvPe1j9IU56vr4ZTScbDSH2OF1NX4Zrh6qUGD67Qd0-QHvYV8Dgyg-BuFSenUgXwsNF1I4W15Sc_dteR4Um2enCj8SEGEpOm7vtnrCq2MGKcOETgFOxUQpYb7co0gubVIH9oByjkCuNyNLAUp0GO-3MP-Ev4BiqTqWY.SIfMYBUStapbl_nM2Ba1OTG9fvmyZzmUVabAzWcHSmA&dib_tag=se&keywords=dessicator%2Bbeads&qid=1769119978&sprefix=dessicator%2Bbeads%2Caps%2C105&sr=8-3&th=1">Silica Desiccants Beads</a> |
| 3 mm ID Silicon Tubing | 2 rolls | <a href="https://www.adafruit.com/product/4661">Silicone Tubing for Air Pumps and Valves</a> | 
| Air pumps | 2 | <a href="https://www.adafruit.com/product/4699">Adafruit Air Pump and Vacuum DC Motor - 4.5 V and 2.5 LPM</a> | 
| Microcontroller | 1 | <a href="adafruit.com/product/3500?srsltid=AfmBOopctgbF7qVH7y2xIG2ivOXZ61lkIyijYA6622Jabe7w3e4Oubws">Adafruit Trinket M0</a> | 
| Humidity Sensor | 1 | <a href="https://www.adafruit.com/product/2857?srsltid=AfmBOoqs4Np-P6ohPI5jEt_gJOYip02rR1GwN0GPTHiV_GRCL9Y_RTrb">Adafruit SHT31 Humidity Sensor</a> | 
| Potentiometer | 1 | <a href="https://www.adafruit.com/product/4272">10 kΩ potentiometer</a> | 
| Breadboard | 2 | <a href="https://www.adafruit.com/product/1609">Adafruit Breadboard</a> | 
| Heating Pad | 2 | <a href="https://www.adafruit.com/product/1481?gad_source=1&gad_campaignid=21079227318&gbraid=0AAAAADx9JvR97CeB5cWs3SDTGd6_GFXD-&gclid=Cj0KCQiAhtvMBhDBARIsAL26pjG0pcvWwEmo3NjDLNvYbbM8M9ByCQaJ4RD1xlkxkuu-bqjk-bu4iRkaAj8AEALw_wcB">Electric Heating Pad - 10cm x 5cm </a> | 



  </p>
  </section>

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
  </section>

  <!-- Footer -->
  <footer class="text-center py-3" style="background-color: #1e6f5c; color: white;">
    <p>&copy; 2025 CBE3300B | Designed by Paulina Bargallo</p>
  </footer>
</body>
</html>
Milestone4.html" target="_blank">
    🎯 Milestone 4
  </a>
</li>


    <div class="tab-content" id="milestoneTabsContent">
      <!-- Keep Milestone 4 tabs and content untouched -->
      <div class="tab-pane fade show active" id="milestone4" role="tabpanel" aria-labelledby="milestone4-tab">
        <!-- Your existing Milestone 4 content here -->
      </div>

      <div class="tab-pane fade" id="milestone4-pot" role="tabpanel" aria-labelledby="milestone4-pot-tab">
        <!-- Your existing Milestone 4 Potentiostat content here -->
      </div>
    </div>
  </section>
  
  <!-- Footer -->
  <footer class="text-center py-3" style="background-color: #1e6f5c; color: white;">
    <p>&copy; 2025 CBE3300B | Designed by Paulina Bargallo</p>
  </footer>
</body>
</html>
