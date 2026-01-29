# CBE-3300B

# Project: 

In this project, using an air pump and a controller, the humidity of a chamber is controller to various set points. Using an air pump, air is run through a flask of water and a flask with dessicator beads. As air is bubbled through the water, the outlet stream of the flask delivers humid air to the chamber. As needed, air is run through the flask with dessicator beads to deliver dry air. Based on a set point, a sensor reads the humidity within the chamber, with the signal sent to a controller that adjusts the flow of humid air and dry air coming from the air pump to reach a desired set point. The PFD of the design is shown below. 

<img width="802" height="576" alt="image" src="https://github.com/user-attachments/assets/bc75f81a-391e-46e6-b414-4864870c4b7d" />

# Premise: 

### Physical/Chemical Principles:

In this device, a controller is used to change the flow rates of two air streams to manage the relative humidity (RH) of a chamber. Relative humidity is the ratio of the current humidity to the maximum humidity level air can hold at a specific temperature and pressure. This device will run at atmospheric pressure and the chamber will likely be at around room temperature, due to likely negligible heat being transferred from the humid air stream to the chamber.

The RH of the chamber is controlled by air streams from two air pumps. Two streams are mixed and delivered to the chamber: a dry air stream with negligible water vapor content and a humidified air stream formed by bubbling air through water. By adjusting the relative flow rates of these streams, the ratio of humid air to dry air increases or decreases, which increases or decreases the RH of the chamber. The water is being kept at a constant temperature to ensure that the RH of the humid air input stream remains constant; in this way, only adjustment of the flow rate changes the RH of the chamber. Another level of RH control of the chamber can come from changing the temperature of the water. If the water's temperature increases, the humidity of the input humid stream also increases, making drastic increases to the RH of the chamber possible.

There are no reactions in this device. There are no corrosive materials either. 

### Calculations

There are no calculations necessary. We will be using a PID algorithm and the humidity sensor automatically calculates the RH. 

### Prototyping Approach
The proposed prototype is a benchtop humidity-controlled chamber that regulates relative humidity by mixing two independently generated air streams: a dry air stream and a humidified air stream. Each stream is driven by a dedicated air pump, allowing independent control of airflow rates and precise adjustment of humidity entering the chamber.

The control error is defined as the difference between the desired humidity setpoint and the measured chamber humidity. The PID controller computes a control signal based on the instantaneous error, the accumulated error over time, and the rate of change of the error. This combined response enables fast correction of deviations while minimizing steady-state offset and oscillations. The control unit we will be using is the Adafruit control unit (Adafruit Trinket M0) which can be controlled using Python. The microcontroller here will be using pulse width modulation, or PWM, to control the air pumps. PWM allows for the air pumps to be rapidly turned on and off, which allows for close control of the humid and dry air streams, preventing large overshoot. Using a PID algorithm also helps prevent over/undershoot. 

The input to the controller is the reading from the humidity sensor (Adafruit Sensirion SHT31-D). This humidity controller directly measures the RH of the chamber and can be connected to the Trinket M0 using the pin-out from the manual. Thus the flow of the electrical signal goes from humidity controller to Adafruit microcontroller to air pump(s). 


# Components: 

_Mechanical_
- Chamber (32oz: https://tinyurl.com/cbe3300container32oz, square container: )
- 2 flasks - one for humid air with aquarium beads, one for the dessicator with dry air.
- Dessicator beads (https://www.amazon.com/Premium-Indicating-Silica-Industry-Standard/dp/B013L31PQ0/ref=sr_1_3?crid=RVISUUHO3UJ0&dib=eyJ2IjoiMSJ9._DjEw0TeT2_ClWLr2xpFAjEGwNT1-fvj-2YUOqST7cjMp37gEEVuNHrUyKnISaTJPYOsKh0TMvPe1j9IU56vr4ZTScbDSH2OF1NX4Zrh6qUGD67Qd0-QHvYV8Dgyg-BuFSenUgXwsNF1I4W15Sc_dteR4Um2enCj8SEGEpOm7vtnrCq2MGKcOETgFOxUQpYb7co0gubVIH9oByjkCuNyNLAUp0GO-3MP-Ev4BiqTqWY.SIfMYBUStapbl_nM2Ba1OTG9fvmyZzmUVabAzWcHSmA&dib_tag=se&keywords=dessicator%2Bbeads&qid=1769119978&sprefix=dessicator%2Bbeads%2Caps%2C105&sr=8-3&th=1) 
- Tubing - bendable silicone tubes (2 rolls of 3mm ID: https://www.adafruit.com/product/4661, 2 rolls of ~3.175mm ID: https://tinyurl.com/tubingforcbe3300b)
- Gas push tube sealing (https://www.sp-spareparts.com/en/p/qsq-6-4-festo) 
- 2 Air pumps (Adafruit Air Pump and Vacuum DC Motor - 4.5 V and 2.5 LPM - ZR370-02PM: https://www.adafruit.com/product/4699) 

_Electrical_
- Hotplate
- Adafruit control unit (adafruit.com/product/3500?srsltid=AfmBOopctgbF7qVH7y2xIG2ivOXZ61lkIyijYA6622Jabe7w3e4Oubws) 
- Humidity sensor (https://www.adafruit.com/product/2857?srsltid=AfmBOoqs4Np-P6ohPI5jEt_gJOYip02rR1GwN0GPTHiV_GRCL9Y_RTrb)
- PN2222 transistor x2
- 220 Ω resistor x3
- 1N4001 diode x2
- 10 kΩ potentiometer
- LED
