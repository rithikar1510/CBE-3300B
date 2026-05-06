This page covers what future directions of the device, including both additional features and improvements to the current product. 

# Improvements 
## Alternative Control Strategies
The current MVP uses hybrid control. When the error (the difference between the humidity reading and the set point) is less than 4%, then proportional control turns on. The duty cycle 
is changed in proportion to the error's magnitude. However, when error is above 4%, then bang bang/on and off control is used. This combination works reasonably well,
with a short settling time (roughly 20 seconds) and even oscillations around the set point (see MVP page). However, we noticed that at lower RH set points, minimizing error was 
very difficult. This is likely because since RH is relative, at lower values, a small jump in humidity leads to larger magnitudes of change in RH. For example, if the RH is ner 80%,
a low flowrate of constant humidity air doesn't change the value much because the RH is already near saturation. However, at low RH values, the same amount of constant humidity air
creates large jumps because there is a larger relative change. 

A solution could be to change the control logic depending on which direction RH must move. For example, if the disturbance (i.e. change in set point) is increasing the RH set point, then
a specific control logic is used, but if the disturbance is shrinking the RH set point, then the control logic allows for smaller incremental changes of the humid air line's duty cycle. However,
PI or PID control would likely improve this. The controller used does have the capacity to do PID control, so significant hardware change wouldn't be needed. PID control would allow for more adaptive 
ability for different disturbance variables. However, it would increase the processing power used.

# Hardware Changes
## Pump Choice 
The current device used a 32oz/~0.95 L container. However, this could be replaced with a bigger container for more applications simply, but the settling time likely would
also increase. Larger pumps would need to be used. The current pumps have a maximum capacity of 2.5 LPM, so to keep the settling time under 30 seconds, larger pumps that would 
drive up the cost would be needed. Bigger pumps also would allow for differences in power. For the same change in duty cycle, more flow rates are possible, allowing for tighter tuning. 

## Heating Element
Many industrially available climatic test chambers pair humidity and temperature control. Temperature control could be added using electric heating pads ($5.95 at Adafruit). This can be connected to the breadboard and the SHT31d sensor 
also has temperature capacity. However, this introduced a safety element because insulating material would have to be added outside of the heating pad. Else, the user could burn themselves.
Also the container would have to be made of something that wouldn't melt - currently the container is made out of plastic. At high temperatures, this could lead to melting.

A heating pad could also be added around the water resevoir. This would increase the Psat of the water, which would increase the maximum humidity of the air line. However, there is the same risk of plastic melting and safety, 
especially since the water resevoir is housed in the 3D printed case. Since the maximum RH set point is around 85%, adding a heating component would allow for higher maximum RH. However, it might lead to more overshoot, 
requiring change of the control logic. 

## Display Size
The current display used is 128x128 pixels. It is quite small to see. Using a bigger display would make the device more user friendly. Furthermore, it would be helpful to more explicitly map the set points to changing the slde on the 10k potentiometer. Both of these changes would make the user interface must more clear. 

Use bigger display and include switch for user friendliness
