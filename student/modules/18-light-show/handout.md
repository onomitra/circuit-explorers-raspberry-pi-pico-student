# NeoPixel Light Show Cheat Sheet — Module 18

## Wiring: NeoPixel Strip (8 LEDs)

```
  Pico GP15  (pin 20) ──── NeoPixel DIN (Data In)
  Pico VBUS  (pin 40) ──── NeoPixel VCC (5V Power)
  Pico GND   (pin 38) ──── NeoPixel GND
```

**Important:** Power goes to **VBUS** (5V), NOT 3.3V! Connect to the **DIN** end, not DOUT.

## NeoPixel Setup

```python
import neopixel
from machine import Pin

NUM_LEDS = 8
np = neopixel.NeoPixel(Pin(15), NUM_LEDS)
```

## Setting Pixel Colors

```python
np[0] = (255, 0, 0)      # Set pixel 0 to red
np[3] = (0, 255, 0)      # Set pixel 3 to green
np.write()                # Send colors to the strip!
```

**Nothing changes until you call `np.write()`!**

Pixels are numbered **0 to 7** (not 1 to 8).

## Color Tuples (R, G, B) — Values 0-255

| Color | Tuple |
|-------|-------|
| Red | `(255, 0, 0)` |
| Green | `(0, 255, 0)` |
| Blue | `(0, 0, 255)` |
| Yellow | `(255, 255, 0)` |
| Cyan | `(0, 255, 255)` |
| Purple | `(128, 0, 255)` |
| Orange | `(255, 165, 0)` |
| White | `(255, 255, 255)` |
| Off | `(0, 0, 0)` |

**Tip:** Keep values under 50 to avoid blinding brightness!

## Clear All Pixels

```python
def clear():
    for i in range(NUM_LEDS):
        np[i] = (0, 0, 0)
    np.write()
```

## Color Wheel (Rainbow Helper)

```python
def wheel(pos):
    """Convert 0-255 to a rainbow color."""
    if pos < 85:
        return (255 - pos * 3, pos * 3, 0)
    elif pos < 170:
        pos -= 85
        return (0, 255 - pos * 3, pos * 3)
    else:
        pos -= 170
        return (pos * 3, 0, 255 - pos * 3)
```

## Animation Pattern

```python
while True:
    # 1. Set pixel colors
    for i in range(NUM_LEDS):
        np[i] = some_color
    # 2. Send to strip
    np.write()
    # 3. Wait
    time.sleep(0.05)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **NeoPixel** | Brand name for WS2812 addressable RGB LEDs |
| **WS2812** | The chip inside each NeoPixel that receives data and controls the RGB LED |
| **Addressable LED** | An LED with its own chip so it can be controlled individually on a shared wire |
| **RGB** | Red, Green, Blue — mixed to make any color (values 0-255 each) |
| **Pixel** | One LED on the strip, numbered starting from 0 |
| **`np.write()`** | Sends color data to the strip — nothing changes without it |
| **Color wheel** | A function that maps 0-255 to a smooth rainbow color |
| **Animation** | Rapidly changing colors in a pattern to create the illusion of movement |

---
*Circuit Explorers — Module 18*
