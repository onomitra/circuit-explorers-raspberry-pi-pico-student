# PWM & Breathing Light Cheat Sheet — Module 5

## Wiring (same as Module 2)

```
  GP15 ──── [220Ω Resistor] ──── LED (+) ──── LED (-) ──── GND
```

## PWM Setup

```python
from machine import Pin, PWM

pwm = PWM(Pin(15))       # Create PWM on GP15
pwm.freq(1000)           # 1000 cycles per second
pwm.duty_u16(32768)      # Set brightness (0-65535)
```

## Duty Cycle Values

| Value | Brightness |
|-------|-----------|
| `0` | Off (0%) |
| `16384` | Dim (25%) |
| `32768` | Medium (50%) |
| `49152` | Bright (75%) |
| `65535` | Full (100%) |

## Duty Cycle Explained

```
  100% ON:  ████████████████   (full brightness)
   50% ON:  ████████░░░░░░░░   (half brightness)
   25% ON:  ████░░░░░░░░░░░░   (quarter brightness)
     0% ON:  ░░░░░░░░░░░░░░░░   (off)
```

Repeats 1000x per second — too fast to see the flicker!

## Breathing LED Pattern

```python
from machine import Pin, PWM
import time

pwm = PWM(Pin(15))
pwm.freq(1000)

while True:
    for brightness in range(0, 65535, 256):       # Up
        pwm.duty_u16(brightness)
        time.sleep(0.005)
    for brightness in range(65535, -1, -256):     # Down
        pwm.duty_u16(brightness)
        time.sleep(0.005)
```

## range() Quick Reference

```python
range(start, stop, step)

range(0, 65535, 256)      # 0, 256, 512, ... 65280
range(65535, -1, -256)    # 65535, 65279, ... 0
```

`range()` stops BEFORE the stop value!

## Vocabulary

| Word | Meaning |
|------|---------|
| **PWM** | Pulse Width Modulation — fast on/off to fake brightness levels |
| **Duty cycle** | Percentage of time the signal is ON |
| **Frequency** | How many on/off cycles per second (Hz) |
| **Analog** | A signal with many possible values (like brightness) |
| **Digital** | A signal with only two values: on (1) or off (0) |
| **`duty_u16()`** | Sets duty cycle: 0 (off) to 65535 (fully on) |

---
*Circuit Explorers — Module 5*
