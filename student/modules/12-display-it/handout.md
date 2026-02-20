# OLED Display Cheat Sheet — Module 12

## Wiring: SSD1306 OLED (I2C, 4-pin)

```
  Pico 3.3V (pin 36) ──── OLED VCC
  Pico GND  (pin 38) ──── OLED GND
  Pico GP0  (pin 1)  ──── OLED SDA
  Pico GP1  (pin 2)  ──── OLED SCL
```

**Important:** VCC goes to **3.3V**, NOT 5V! Check the labels on your OLED module — pin order varies by manufacturer.

## I2C Setup

```python
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C

i2c = I2C(0, sda=Pin(0), scl=Pin(1))
oled = SSD1306_I2C(128, 64, i2c)
```

## I2C Scan (Find Devices)

```python
i2c = I2C(0, sda=Pin(0), scl=Pin(1))
devices = i2c.scan()
print("Found:", devices)    # Should show [60] (0x3C)
```

## Display Commands

| Command | What It Does |
|---------|-------------|
| `oled.text("Hello", x, y)` | Draw text at position (x, y) |
| `oled.show()` | Send drawing to the screen (required!) |
| `oled.fill(0)` | Clear the screen (fill with black) |
| `oled.fill(1)` | Fill the screen with white |
| `oled.invert(True)` | Invert colors (white becomes black) |
| `oled.rect(x, y, w, h, 1)` | Draw a rectangle outline |

## Text Positioning

```
  Screen: 128 pixels wide x 64 pixels tall
  Each character: 8 pixels wide x 8 pixels tall
  Max characters per line: 16
  Max lines: 8

  Line 1: y = 0
  Line 2: y = 8
  Line 3: y = 16
  Line 4: y = 24
  ...
```

## Update Pattern (Clear, Draw, Show)

```python
while True:
    oled.fill(0)                  # 1. Clear
    oled.text("Count: " + str(n), 0, 0)  # 2. Draw
    oled.show()                   # 3. Show
    n += 1
    time.sleep(1)
```

**Remember:** Numbers must be converted to strings with `str()` before displaying!

## Vocabulary

| Word | Meaning |
|------|---------|
| **I2C** | Communication protocol using 2 wires (SDA + SCL) to talk to devices |
| **SDA** | Serial Data line — carries the actual data |
| **SCL** | Serial Clock line — keeps the timing synchronized |
| **Address** | Unique number for each I2C device (OLED = 0x3C / 60) |
| **OLED** | Organic Light-Emitting Diode — each pixel makes its own light |
| **SSD1306** | The controller chip inside the OLED display module |
| **Library** | A file of pre-written code that adds new features (`ssd1306.py`) |
| **Pixel** | The smallest dot on a screen — the display is 128 x 64 pixels |

---
*Circuit Explorers — Module 12*
