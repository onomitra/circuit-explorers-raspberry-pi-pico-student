# Circuit Building Cheat Sheet — Module 2

## How a Breadboard Works

```
  (+) (-)    a  b  c  d  e     f  g  h  i  j    (+) (-)
   │   │   ┌─────────────┐   ┌─────────────┐     │   │
   │   │  1│ ●──●──●──●──● │ │ ●──●──●──●──● │   │   │
   │   │  2│ ●──●──●──●──● │ │ ●──●──●──●──● │   │   │
   │   │  3│ ●──●──●──●──● │ │ ●──●──●──●──● │   │   │
   ▼   ▼   └─────────────┘   └─────────────┘     ▼   ▼
  runs down  ◄─connected─►     ◄─connected─►    runs down
```

- Holes in the **same row** (a-e or f-j) are connected
- The **center gap** breaks the connection
- **Power rails** (+/-) run top to bottom

## LED Polarity — Get It Right!

```
  Long leg = + (anode)      Short leg = - (cathode)
       │                          │
       │        ┌───┐             │
       └────────│ ● │─────────────┘
                └───┘
            flat side = -
```

Put it in backwards = nothing happens. Check the legs!

## Module 2 Circuit

```
  GP15 ── wire ── resistor ── LED (+) ── LED (-) ── wire ── GND
```

## Pre-Power Checklist

Before plugging in USB, verify:
- [ ] LED long leg (+) and resistor are in the **same row**
- [ ] LED short leg (-) and GND wire are in the **same row**
- [ ] Wire from Pico **GP15** to the resistor's row
- [ ] Wire from Pico **GND** to the LED's short leg row

## Key Code

```python
from machine import Pin
import time

# Set up external LED on GPIO pin 15
led = Pin(15, Pin.OUT)

# Turn on / off
led.value(1)    # ON
led.value(0)    # OFF
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **Circuit** | A complete loop that electricity flows through |
| **Breadboard** | A board for building circuits without soldering |
| **Resistor** | Limits current to protect components |
| **Anode (+)** | Positive side of LED — long leg |
| **Cathode (-)** | Negative side of LED — short leg |
| **Voltage** | The "push" behind electricity (Volts) |
| **Current** | How much electricity flows (Amps) |
| **Resistance** | How much flow is slowed down (Ohms) |

---
*Circuit Explorers — Module 2*
