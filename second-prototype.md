---
layout: default
title: Second Prototype
---

## Second Prototype Prototype

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

SDA - white, SCL - orange

