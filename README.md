# CBE-3300B

# Project: 

In this project, using an air pump and a controller, the humidity of a chamber is controller to various set points. Using an air pump, air is run through a flask of water and a flask with dessicator beads. As air is bubbled through the water, the outlet stream of the flask delivers humid air to the chamber. As needed, air is run through the flask with dessicator beads to deliver dry air. Based on a set point, a sensor reads the humidity within the chamber, with the signal sent to a controller that adjusts the flow of humid air and dry air coming from the air pump to reach a desired set point. The PFD of the design is shown below. 

<img width="802" height="576" alt="image" src="https://github.com/user-attachments/assets/bc75f81a-391e-46e6-b414-4864870c4b7d" />

# Premise: 

### Physical/Chemical Principles:

In this device, a controller is used to change the flow rates of two air streams to manage the relative humidity (RH) of a chamber. Relative humidity is the ratio of the current humidity to the maximum humidity level air can hold at a specific temperature and pressure. This device will run at atmospheric pressure and the chamber will likely be at around room temperature, due to likely negligible heat being transferred from the humid air stream to the chamber.

The RH of the chamber is controlled by air streams from two air pumps. Two streams are mixed and delivered to the chamber: a dry air stream with negligible water vapor content and a humidified air stream formed by bubbling air through water kept at a specific temperature. By adjusting the relative flow rates of these streams, the ratio of humid air to dry air increases or decreases which increases or decreases the RH of the chamber. The water is being kept at a constant temperature to ensure that the RH of the humid air input stream remains constant; in this way, only adjustment of the flow rate changes the RH of the chamber. Another level of RH control of the chamber can come from changing the temperature of the water. If the water's temperature increases, the humidity of the input humid stream also increases, making drastic increases to the RH of the chamber possible.

### Calculations
### Prototyping Approach
The proposed prototype is a benchtop humidity-controlled chamber that regulates relative humidity by mixing two independently generated air streams: a dry air stream and a humidified air stream. Each stream is driven by a dedicated air pump, allowing independent control of airflow rates and precise adjustment of humidity entering the chamber.


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
- Controller
- Adafruit control unit 
- Humidity sensor
