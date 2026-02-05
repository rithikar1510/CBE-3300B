# CBE-3300B

# Project: 

In this project the humidity of a chamber is controlled to various set points. Using an air pump, air is run through a flask of water and a flask with dessicator beads. As air is bubbled through the water, the outlet stream of the flask delivers humid air to the chamber. As needed, air is run through the flask with dessicator beads to deliver dry air. Based on a set point, a sensor reads the humidity within the chamber, with the signal sent to a controller that adjusts the flow of humid air and dry air coming from the air pump to reach a desired set point. The PFD of the design is shown below. 

<img width="958" height="674" alt="image" src="https://github.com/user-attachments/assets/4c2dd152-7333-41a0-9a31-7b6c79f1536a" />

# Premise: 

### Physical/Chemical Principles:

In this device, a controller is used to change the flow rates of two air streams to manage the relative humidity (RH) of a chamber. Relative humidity is the ratio of the current humidity to the maximum humidity level air can hold at a specific temperature and pressure. This device will run at atmospheric pressure and the chamber will likely be at around room temperature, due to likely negligible heat being transferred from the humid air stream to the chamber.

The RH of the chamber is controlled by air streams from two air pumps. Two streams are mixed and delivered to the chamber: a dry air stream with negligible water vapor content and a humidified air stream formed by bubbling air through water. By adjusting the relative flow rates of these streams, the ratio of humid air to dry air increases or decreases, which increases or decreases the RH of the chamber. The water is being kept at a constant temperature to ensure that the RH of the humid air input stream remains constant; in this way, only adjustment of the flow rate changes the RH of the chamber. Another level of RH control of the chamber can come from changing the temperature of the water. If the water's temperature increases, the humidity of the input humid stream also increases, making drastic increases to the RH of the chamber possible.

There are no reactions in this device. There are no corrosive materials either. 

### Calculations

There are no calculations necessary. We will be using a PID algorithm and the humidity sensor automatically calculates the RH. 

#### Summary:
The Arduino acts as the central controller for the system and provides power and control signals to the circuit. All components share a common ground so they can communicate properly. A humidity sensor inside the chamber continuously measures the relative humidity and sends this information to the Arduino. The desired humidity level is set using a potentiometer, which the Arduino reads as an input. Since the air pumps require more power than the Arduino can supply directly, they are controlled using transistor switch circuits that allow the Arduino to safely turn the pumps on and off. Diodes are included across the pumps to protect the circuit when the pumps are switched off. An LED is used as a simple visual indicator to show when the system is operating. Together, these components allow the system to monitor and adjust humidity in the chamber in a controlled and reliable way.

### Prototyping Approach
The proposed prototype is a benchtop humidity-controlled chamber that regulates relative humidity by mixing two independently generated air streams: a dry air stream and a humidified air stream. Each stream is driven by a dedicated air pump, allowing independent control of airflow rates and precise adjustment of humidity entering the chamber.

#### Controller

The control error is defined as the difference between the desired humidity setpoint and the measured chamber humidity. The PID controller computes a control signal based on the instantaneous error, the accumulated error over time, and the rate of change of the error. This combined response enables fast correction of deviations while minimizing steady-state offset and oscillations. The control unit we will be using is the Adafruit control unit (Adafruit Trinket M0) which can be controlled using Python. The microcontroller here will be using pulse width modulation, or PWM, to control the air pumps. PWM allows for the air pumps to be rapidly turned on and off, which allows for close control of the humid and dry air streams, preventing large overshoot. Using a PID algorithm also helps prevent over/undershoot. 

The input to the controller is the reading from the humidity sensor (Adafruit Sensirion SHT31-D). This humidity controller directly measures the RH of the chamber and can be connected to the Trinket M0 using the pin-out from the manual. Thus the flow of the electrical signal goes from humidity controller to Adafruit microcontroller to air pump(s). A nondigital, manual input can also be added using a potentiometer, where the voltage can be varied to vary the desired set point of the microcontroller. The potentiometer can be connected to an analog input on the Trinket M0, which can then be read in the code to vary the set point. 

#### Circuit Description
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

# Components: 

_Mechanical_
- Chamber (32oz: https://tinyurl.com/cbe3300container32oz, square container: https://tinyurl.com/othercontainer)
- 2 flasks - one for humid air with aquarium beads, one for the dessicator with dry air. (https://www.indigoinstruments.com/glassware/erlenmeyer_flasks/500ml-erlenmeyer-flask-55207.html?srsltid=AfmBOopTUBJz1xRSZRwkczvqmDJVzZRJL-TcLrCXVP2Cq8P8r6OROCOW)
- Plastic vials https://www.amazon.com/Plastic-Containers-Tubes-Sample-Container/dp/B01N9U2DS1/ref=sr_1_6?crid=3GUXLGM9JGLI9&dib=eyJ2IjoiMSJ9.a5FOY3fwxTpOPNHUqYt9sEkz7lpp3faPnjcQrS_nAqdcq5FJHd1bI8hD21D7T--_BCkX8AU54NjSZq553PBbF5Dyd0IRmsv_2Y1jIVA5cs_K9BimaUjY15HiYJWaUfMdnWJzlxDRFr0nx7UIG7sKC35Iuey89kE1csafQJk_FalLBle1xPvsgAz8crYNbbz4gSTJfm14MmYUn0nK5ZOZemhrzETkUN1yd--8cjm_xDVNVasmGOb1lvzXqAuwnVWEjFUwUX4yGO2hfkWF2AnxYYdPPBWFRf3hlViYJX_Ylnk.JhqsE9nffH_L9MkHEhtS3HWSHV_5lZNoTfL3PWa0L-Q&dib_tag=se&keywords=plastic+vials&qid=1769724743&s=industrial&sprefix=plastic+vials+%2Cindustrial%2C105&sr=1-6
- PLastic vials (second option) https://www.amazon.com/ASEVAT-Leak-proof-Container-Translucent-Containers/dp/B0DSP2WHTW/ref=sr_1_10_sspa?crid=3GUXLGM9JGLI9&dib=eyJ2IjoiMSJ9.JCFmWnBryUBxMzInTekbPmtgNwl72-cKg6FuGYImiB-_aKz8iKfssCPKzVd3gqb_Id1VPtukAdbMhwt8qb_57fkYIyeJ9PkAphm6OQOa_R5fiMmKDQq_nrT8Dpqq61jdk-cIqxgM1HRBleR3eDLIm3BTtYHzjv9kPdqfda1g9jzy_bexMM_D-DdhXdr6Eh1njzrBNQBtVh9Vm-iZRjCWCr5PFz1vT93xT3GFC0JxWjuA-_VgsBnZEcZY-b_FKJcPx4OpF3kbGe8fCYF7ZyXTi_zT4EyjWfuSzanB_6mmRYo.EcwL_ghv7Aq6NKJX0SNyQh-HNkv2rkxLvBeKLHkOi5o&dib_tag=se&keywords=plastic%2Bvials&qid=1769724905&s=industrial&sprefix=plastic%2Bvials%2B%2Cindustrial%2C105&sr=1-10-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9tdGY&th=1
- Dessicator beads (https://www.amazon.com/Premium-Indicating-Silica-Industry-Standard/dp/B013L31PQ0/ref=sr_1_3?crid=RVISUUHO3UJ0&dib=eyJ2IjoiMSJ9._DjEw0TeT2_ClWLr2xpFAjEGwNT1-fvj-2YUOqST7cjMp37gEEVuNHrUyKnISaTJPYOsKh0TMvPe1j9IU56vr4ZTScbDSH2OF1NX4Zrh6qUGD67Qd0-QHvYV8Dgyg-BuFSenUgXwsNF1I4W15Sc_dteR4Um2enCj8SEGEpOm7vtnrCq2MGKcOETgFOxUQpYb7co0gubVIH9oByjkCuNyNLAUp0GO-3MP-Ev4BiqTqWY.SIfMYBUStapbl_nM2Ba1OTG9fvmyZzmUVabAzWcHSmA&dib_tag=se&keywords=dessicator%2Bbeads&qid=1769119978&sprefix=dessicator%2Bbeads%2Caps%2C105&sr=8-3&th=1) 
- Tubing - bendable silicone tubes (2 rolls of 3mm ID: https://www.adafruit.com/product/4661, 2 rolls of ~3.175mm ID: https://tinyurl.com/tubingforcbe3300b)
- Gas push tube sealing (https://www.sp-spareparts.com/en/p/qsq-6-4-festo) 
- 2 Air pumps (Adafruit Air Pump and Vacuum DC Motor - 4.5 V and 2.5 LPM - ZR370-02PM: https://www.adafruit.com/product/4699)

_Electrical_
- Hotplate
- Heating tape (to replace the hotplate)
- Adafruit control unit (adafruit.com/product/3500?srsltid=AfmBOopctgbF7qVH7y2xIG2ivOXZ61lkIyijYA6622Jabe7w3e4Oubws) 
- Humidity sensor (https://www.adafruit.com/product/2857?srsltid=AfmBOoqs4Np-P6ohPI5jEt_gJOYip02rR1GwN0GPTHiV_GRCL9Y_RTrb)
- PN2222 transistor x2 (https://www.adafruit.com/product/756?srsltid=AfmBOoo7C42COf9H8JbmK_CzWmIN2YvXj0G7fIRqkkr9lZ8j5g4WMziS) 
- 220 Ω resistor x3 (https://www.adafruit.com/product/2780?srsltid=AfmBOorU7fWfvjgyQ9G5W5X2wKHUn3kKfnXNEA0QN95GRun7J6XIEkm8) 
- 1N4001 diode x2 (https://www.adafruit.com/product/755?srsltid=AfmBOoqwnaH7m_vs5eA3YAm43x0dlSPF-3HmRYoi4unNG2oGFVwZbGND) 
- 10 kΩ potentiometer (https://www.adafruit.com/product/4272) 
- Breadboard (https://www.adafruit.com/product/1609) 
