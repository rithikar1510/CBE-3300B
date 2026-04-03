---
layout: default
title: Second Prototype
---

# Second Prototype Prototype

This section goes over the various checkpoints and updates as the initial prototype is being updated for the second demonstration. 

## Checkpoint 3/31/26

Today the goal was to implement a PWM algorithm to modulate the air pumps more than simple off and on. 

### Attempting PWM Control

This code was intended to be able to see if half power could be achieved with a pump. Here the pump turns on at full power, turns off, then turns on for half power before turning back off. 

```python
import time
import board
import pwmio

# Create PWM outputs on D3 and D4
pump1 = pwmio.PWMOut(board.D3, frequency=1000, duty_cycle=0)
pump2 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0)

# duty_cycle ranges from 0 to 65535
OFF = 0
HALF = 32768
FULL = 65535

while True:
    # pump 1 full on for 2 seconds
    pump1.duty_cycle = FULL
    time.sleep(2)

    # pump 1 off
    pump1.duty_cycle = OFF
    time.sleep(1)

    # pump 1 half power for 2 seconds
    pump1.duty_cycle = HALF
    time.sleep(2)

    # pump 1 off
    pump1.duty_cycle = OFF
    time.sleep(1)
```

### Using PWM Control for Narrower Uncertainty

The goal here was to make the pump turn to half power if the humidity is less than 2% away from the intended threshold. For example, in the below code, the HIGH_THRESHOLD was set to 55% and LOW_THRESHOLD was set to 50%. 
What this means for the control is that if the RH is significantly below the intended range (i.e. 50-55%), then the desicator air pump (D4) turns on at full power. But if the difference between the ideal range and the
actual reading is less than 2% (i.e. only a little change should occur), the air pump switches to half power to prevent overshoot. The same happens at the higher range; if the measured RH is significantly above the 
HIGH_THRESHOLD, the humid air pump (D3) turns on at full power. If RH_measured is only 2% away from the desired threshold, then only half the power is used.

```python
import time
import board
import pwmio
import busio
import adafruit_sht31d

# PWM outputs
pump1 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0) #dry
pump2 = pwmio.PWMOut(board.D3, frequency=1000, duty_cycle=0) #humid

# Sensor setup
i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

HIGH_THRESHOLD = 55
LOW_THRESHOLD = 50

# duty_cycle values
OFF = 0
HALF = 32768
FULL = 65535

humidity = sensor.relative_humidity

while humidity != None:
    
    humidity = sensor.relative_humidity
    print((humidity, HIGH_THRESHOLD, LOW_THRESHOLD))
    
    if humidity > HIGH_THRESHOLD:
        pump2.duty_cycle = OFF  # ensure other pump is off
        if (humidity - HIGH_THRESHOLD) < 2:
            pump1.duty_cycle = HALF
        else:
            pump1.duty_cycle = FULL
    # LOW humidity → pump2
    elif humidity < LOW_THRESHOLD:
        pump1.duty_cycle = OFF  # ensure other pump is off
        if (LOW_THRESHOLD - humidity) < 2:
            pump2.duty_cycle = HALF
        else:
            pump2.duty_cycle = FULL
    else: 
        pump1.duty_cycle = OFF
        pump2.duty_cycle = OFF
        
    humidity = sensor.relative_humidity
    time.sleep(2)
```

## Checkpoint 4/2/2026

### Hardware

Today we soldered the board such that the Trinket was attached to a breadboard, with the MOSFETs connected via STEMMA QT wire. We soldered the ground wires and left the positive terminal of the battery. We also soldered the wires to the air pumps. The wires on the SHT31d were also soldered, with the humidity sensor on the inside of the tub with the wires threaded through 4 holes in the container. This was then tested, where we confirmed that the humidity was being toggled correctly! We also moved the tubing on the humid aid line so the hole was at the lid; this removed the sputtering. 

### Overshoot

We realized that the humid air line caused significant overshoot; we lowered the PWM sensing such that at a 2% difference, the power was reduced to 1/4 instead of 1/2. This caused less overshoot. This is the updated code: 

```python
import time
import board
import pwmio
import busio
import adafruit_sht31d
import adafruit_ssd1306

# PWM outputs
pump1 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0)  # dry
pump2 = pwmio.PWMOut(board.D3, frequency=1000, duty_cycle=0)  # humid

# Sensor setup
i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

HIGH_THRESHOLD = 32
LOW_THRESHOLD = 30

# duty_cycle values
OFF = 0
HALF = 32768
QUARTER = 16384
FULL = 65535

while True:
    humidity = sensor.relative_humidity

    # Print humidity clearly
    if humidity is not None:
        print(f"Humidity: {humidity:.2f}%")
    else:
        print("Humidity read failed")

    # Control logic
    if humidity is not None:
        if humidity > HIGH_THRESHOLD:
            pump2.duty_cycle = OFF
            if (humidity - HIGH_THRESHOLD) < 2:
                pump1.duty_cycle = HALF
            else:
                pump1.duty_cycle = FULL

        elif humidity < LOW_THRESHOLD:
            pump1.duty_cycle = OFF
            if (LOW_THRESHOLD - humidity) < 2:
                pump2.duty_cycle = QUARTER
            else:
                pump2.duty_cycle = FULL

        else:
            pump1.duty_cycle = OFF
            pump2.duty_cycle = OFF

    time.sleep(1)
```

### Future Directions

In the future, to try and prevent overshoot, we realized that the humid air line's duty cycle can be reduced only up to ~120000. However, to prevent overshoot, it might be better to change power variably depending on how far the measured RH is from the threshold. Here is the updated code for the next iteration of testing: 

```python
import time
import board
import pwmio
import busio
import adafruit_sht31d
import adafruit_ssd1306

# PWM outputs
pump1 = pwmio.PWMOut(board.D4, frequency=1000, duty_cycle=0)  # dry
pump2 = pwmio.PWMOut(board.D3, frequency=1000, duty_cycle=0)  # humid

# Sensor setup
i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

HIGH_THRESHOLD = 32
LOW_THRESHOLD = 30

# duty_cycle values
OFF = 0
HALF = 32768
QUARTER = 16384
FULL = 65535

while True:
    humidity = sensor.relative_humidity

    # Print humidity clearly
    if humidity is not None:
        print(f"Humidity: {humidity:.2f}%")
    else:
        print("Humidity read failed")

    # Control logic
    if humidity is not None:
        if humidity > HIGH_THRESHOLD:
            pump2.duty_cycle = OFF
            perc_difference = (humidity - HIGH_THRESHOLD)/3
            power_val = perc_difference * FULL 
            if (humidity - HIGH_THRESHOLD) < 3:
                pump1.duty_cycle = power_val
            else:
                pump1.duty_cycle = FULL
        elif humidity < LOW_THRESHOLD:
            pump1.duty_cycle = OFF
            perc_difference = (LOW_THRESHOLD - humidity)/3 
            power_val = perc_difference * FULL 
            if (LOW_THRESHOLD - humidity) < 3:
                pump2.duty_cycle = power_val
            else:
                pump2.duty_cycle = FULL
        else:
            pump1.duty_cycle = OFF
            pump2.duty_cycle = OFF

    time.sleep(1)
```

Trying this code for the next time. We also, upon trying to connect to the display, realized that installing the package required (adafruit_ssd1306) for the display required too much RAM for the Trinket M0. Instead, we are going to try and connect the SHT31d to another Trinket M0 as well (i.e. include the other address for the SHT31d and have it connect to 2 controllers) and connect that controller to the display. This means that we would have to split the signal from the SHT31d; unsure as to whether this will work. As a backup, a Feather has been ordered (because it has much higher RAM) to try and connect to the display. 
