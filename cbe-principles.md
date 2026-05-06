The operation of the ClimaPod humidity control system is based on several fundamental chemical engineering principles that govern moisture transfer, air–water interactions, and system regulation.

# Mass Transfer

Humid and dry air can be delivered through several distinct mechanisms: convective transport, diffusive transport, and adsorption. 

**Convective transport**, specifically forced convective transport, relies on pressure gradients driving flow. For both humid and dry air delivery, we know that the pressure in the chamber is lower than the pressure in the resevoirs. This understanding comes from the outlet hole put into the chamber (see the physical design page); because the chamber is somewhat open to ambient conditions, there is never any pressure build up in the chamber. Thus after either a dried or humidified air stream is created, air flows via silicone tubing to the chamber. For dry air delivery, air movement is driven primarily by advection and pressure gradients created by the pump. Even after passing through desiccator beads, the pressure inside the chamber remains lower than the pump pressure, allowing the pressure drop to sustain airflow into the chamber.

**Diffusive transport** occurs when dry air is bubbled through a liquid. As the air bubbles rise, water vapor evaporates into them until the bubbles approach saturation, or 100% relative humidity. Once the bubbles reach the liquid surface, they burst, releasing vapor that mixes with the surrounding air above the liquid to create a humidified air stream. Relative humidity in this process is described by the ratio of the partial pressure of water vapor to the saturation vapor pressure at a given temperature.

<img width="322" height="128" alt="image" src="https://github.com/user-attachments/assets/4ad52a8b-fb8e-42ec-b645-5ebd2bf2cba0" />​
​
**Adsorption-based drying** uses desiccator beads, which contain many small hydrophilic pores. These pores attract and capture water molecules from the air stream. As air is pumped through the beads, water molecules adhere to the bead surfaces, effectively removing moisture and producing dry air. This process depends on the beads’ porous structure and their affinity for water, making adsorption an effective method for dehumidification.
side the chamber and enables direct comparison to the desired setpoint.

# Controls

Two types of control algorithms were used in this project: bang-bang control and proportional control. This is to balance responsiveness and simplicity. Pumps are regulated using pulse-width modulation (PWM), where the duty cycle adjusts in proportion to the magnitude of the error between the setpoint and the measured value. However, proportional control is only applied when the error is small—specifically, less than 4%. When the error exceeds this threshold, the system switches to bang-bang control, which rapidly toggles the output to drive the system back toward the desired range. This hybrid strategy is chosen primarily for its simplicity and efficiency: it is easy to implement, does not require specialized software libraries, and minimizes computational demands while still maintaining effective control performance.

## Proportional Control
As the system approaches the setpoint, the controller reduces the power supplied to the pumps, resulting in lower airflow rates. This gradual reduction prevents overshooting and allows for stable convergence to the desired humidity. Conversely, when the system is far from the setpoint, higher pump power is applied to increase the rate of humidification or dehumidification. This behavior is characteristic of proportional control, where the control action is scaled based on the magnitude of the error. By modulating the relative flow rates of the humidified and dry air streams, the system ams to have smooth and stable regulation of the RH inside the chamber.

## Bang-Bang Control aka On/Off Control
This control algorithm simply turns the pumps on and off to drive the RH up or down as needed. This method alone creates high overshoot and longer settling times, and uses a lot of power. However, when the setpoint is changed to a value very far from the current humidity, full power and turning off of the other pump can lead to fast changes.
