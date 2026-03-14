# RGB Color Mixing Cheat Sheet — Module 6

## Wiring: RGB LED (Common Cathode)

```
  GP13 ──── [220 Ohm] ──── Red leg
                                 │
  GP14 ──── [220 Ohm] ──── Green leg   ├── RGB LED
                                 │
  GP15 ──── [220 Ohm] ──── Blue leg
                                 │
  GND  ─────────────────── GND leg (longest)
```

**Tip:** The longest leg is the common cathode (GND). Leg order (left to right) is often: Red, GND, Blue, Green — but check your LED!

## PWM Color Setup

```python
from machine import Pin, PWM

red = PWM(Pin(13))
green = PWM(Pin(14))
blue = PWM(Pin(15))
red.freq(1000)
green.freq(1000)
blue.freq(1000)

def set_color(r, g, b):
    red.duty_u16(r)
    green.duty_u16(g)
    blue.duty_u16(b)

set_color(65535, 0, 0)      # Red
set_color(0, 0, 0)          # Off
```

## Color Combination Chart

| Color | Red | Green | Blue |
|-------|-----|-------|------|
| Red | 65535 | 0 | 0 |
| Green | 0 | 65535 | 0 |
| Blue | 0 | 0 | 65535 |
| Yellow | 65535 | 65535 | 0 |
| Cyan | 0 | 65535 | 65535 |
| Magenta | 65535 | 0 | 65535 |
| White | 65535 | 65535 | 65535 |
| Orange | 65535 | 16384 | 0 |
| Off | 0 | 0 | 0 |

## Additive Color Mixing (Light, not paint!)

```
  Red + Green  = Yellow
  Green + Blue = Cyan
  Red + Blue   = Magenta
  All three    = White
```

## Random Colors

```python
import random

r = random.randint(0, 65535)
g = random.randint(0, 65535)
b = random.randint(0, 65535)
set_color(r, g, b)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **RGB** | Red, Green, Blue — the three primary colors of light |
| **Additive color mixing** | Mixing light — more colors = brighter/whiter (opposite of paint!) |
| **Common cathode** | An RGB LED where all three LEDs share the same ground (GND) leg |
| **Function** | A reusable block of code with a name — like `set_color(r, g, b)` |
| **Parameter** | A value you pass into a function — `r`, `g`, and `b` are parameters |
| **`random.randint()`** | Picks a random whole number between two values |

---
*Circuit Explorers — Module 6*
