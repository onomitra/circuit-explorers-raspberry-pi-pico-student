# Button & Input Cheat Sheet — Module 4

## Button Wiring

```
  GP14 ──── wire ──── Button (one side)
                           │
                     [press to connect]
                           │
  GND  ──── wire ──── Button (other side)
```

No external resistor needed — we use `Pin.PULL_UP` in code.

## Pull-Up Behavior

| Button State | `button.value()` |
|-------------|-------------------|
| **Not pressed** | `1` |
| **Pressed** | `0` |

Check for `0` to detect a press!

## if/else Syntax

```python
if button.value() == 0:
    # Button IS pressed — do something
    led.value(1)
else:
    # Button is NOT pressed — do something else
    led.value(0)
```

## Toggle Pattern

```python
led_on = False

while True:
    if button.value() == 0:
        led_on = not led_on      # Flip True/False
        led.value(led_on)
        time.sleep(0.3)          # Debounce!
```

## Booleans

```python
led_on = True       # Yes / On / 1
led_on = False      # No / Off / 0

led_on = not led_on   # Flip: True → False, False → True

# Booleans work as 1/0 in value():
led.value(True)     # Same as led.value(1)
led.value(False)    # Same as led.value(0)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **Input** | Information coming IN to the Pico (button press) |
| **Output** | Information going OUT from the Pico (LED on/off) |
| **Pull-up resistor** | Holds an input pin at `1` until a button pulls it to `0` |
| **Debounce** | Ignoring rapid signals from a button's mechanical bounce |
| **if/else** | Code that makes a decision based on a condition |
| **Toggle** | Switching between two states (on/off) |
| **Boolean** | A value that is either `True` or `False` |

---
*Circuit Explorers — Module 4*
