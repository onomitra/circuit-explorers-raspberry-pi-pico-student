# My First Pico Cheat Sheet — Module 1

## Connecting Your Pico

1. Hold the **BOOTSEL** button (small white button on the board)
2. Plug in the USB cable while holding BOOTSEL
3. Let go of BOOTSEL
4. Open **Thonny** and select **MicroPython (Raspberry Pi Pico)** as the interpreter

## Thonny Quick Reference

| Button | What It Does |
|--------|-------------|
| ▶ Run | Runs your code on the Pico |
| ■ Stop | Stops the running program |
| 💾 Save | Saves your code (save to Pico or to your computer) |

- **Editor** (top) = where you write programs
- **Shell** (bottom) = where you type quick commands and see output

## Basic MicroPython Syntax

```python
# Print a message
print("Hello!")

# Import tools
from machine import Pin
import time

# Set up a pin
led = Pin("LED", Pin.OUT)

# Turn on / off
led.value(1)    # ON
led.value(0)    # OFF

# Wait (in seconds)
time.sleep(0.5)

# Repeat forever
while True:
    # code here runs over and over

# Repeat a set number of times
for i in range(5):
    # code here runs 5 times
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **Microcontroller** | A tiny computer designed to do one specific job |
| **GPIO** | General Purpose Input/Output — the Pico's pins |
| **LED** | Light Emitting Diode — a small light |
| **MicroPython** | Python for small devices like the Pico |
| **REPL** | The Shell in Thonny — type commands one at a time |
| **import** | Load extra tools (modules) for your code |
| **loop** | Code that repeats (`while True` = forever, `for` = set number) |

---
*Circuit Explorers — Module 1*
