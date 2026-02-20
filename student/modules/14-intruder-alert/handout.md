# PIR Motion Sensor Cheat Sheet — Module 14

## HC-SR501 PIR Wiring

```
   PIR Sensor:
   Pico VBUS (5V) ──── wire ──── PIR VCC
   Pico GND       ──── wire ──── PIR GND
   Pico GP14      ──── wire ──── PIR OUT

   Buzzer:
   Pico GP15      ──── wire ──── Buzzer (+) ──── Buzzer (-) ──── GND

   LED:
   Pico GP13      ──── [220 ohm] ──── LED (+) ──── LED (-) ──── GND

   Button (arm/disarm):
   Pico GP12      ──── wire ──── Button ──── GND   (use PULL_UP)
```

**Important:** Check the pin labels on YOUR PIR board — pin order varies! Wait 30-60 seconds after power-on for the sensor to calibrate.

## Reading the PIR Sensor (Polling)

```python
from machine import Pin
import time

pir = Pin(14, Pin.IN)

while True:
    if pir.value() == 1:
        print("Motion detected!")
    else:
        print("All clear.")
    time.sleep(0.5)
```

## Using Interrupts (Smarter Approach)

```python
from machine import Pin

pir = Pin(14, Pin.IN)
motion_detected = False

def motion_handler(pin):
    global motion_detected
    motion_detected = True

pir.irq(trigger=Pin.IRQ_RISING, handler=motion_handler)

# Now check motion_detected in your main loop
```

## Polling vs Interrupts

| | Polling | Interrupts |
|---|---------|-----------|
| **How** | Constantly check in a loop | Hardware notifies you |
| **Analogy** | Looking out the window | Doorbell rings |
| **Efficiency** | Wastes CPU time checking | CPU is free until event |
| **Speed** | Depends on loop delay | Almost instant |
| **Code** | `if pir.value() == 1:` | `pir.irq(handler=func)` |

## PIR Sensor Facts

- **Detection range:** 3-7 meters (adjustable with sensitivity knob)
- **Field of view:** ~110 degrees
- **Warm-up time:** 30-60 seconds after power-on
- **Output:** Digital — HIGH (1) when motion, LOW (0) when clear
- **Detects:** Moving warm objects (people, animals, warm cups)
- **Does NOT detect:** Cold objects, stationary warm objects

## Vocabulary

| Word | Meaning |
|------|---------|
| **PIR** | Passive Infrared — detects moving warm objects by their IR radiation |
| **Infrared** | Invisible light emitted by warm objects, below red in the spectrum |
| **Passive** | Does not send signals — only watches for changes |
| **Fresnel lens** | The white dome that focuses IR light onto the sensor |
| **Polling** | Constantly checking a sensor in a loop |
| **Interrupt** | Hardware notification that runs a function when an event occurs |
| **IRQ_RISING** | Trigger when pin goes from LOW to HIGH |
| **Callback** | A function called automatically by hardware when an event happens |

---
*Circuit Explorers — Module 14*
