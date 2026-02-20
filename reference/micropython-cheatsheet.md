# MicroPython Cheat Sheet for Raspberry Pi Pico 2W

## Basics

```python
# Print to the Shell
print("Hello!")
print(42)
print("Temperature:", temp, "degrees")

# Variables
name = "Pico"
count = 10
temperature = 22.5
led_on = True

# Math
total = 10 + 5      # 15
half = 10 / 2       # 5.0
remainder = 10 % 3  # 1
```

## Control Flow

```python
# If / else
if temperature > 30:
    print("Too hot!")
elif temperature < 10:
    print("Too cold!")
else:
    print("Just right")

# While loop (runs forever)
while True:
    print("Looping!")
    time.sleep(1)

# For loop (runs a set number of times)
for i in range(5):
    print(i)          # prints 0, 1, 2, 3, 4

# For loop with a list
colors = ["red", "green", "blue"]
for color in colors:
    print(color)
```

## Functions

```python
# Define a function
def blink(times):
    for i in range(times):
        led.value(1)
        time.sleep(0.5)
        led.value(0)
        time.sleep(0.5)

# Call it
blink(3)
```

## Common Imports

```python
from machine import Pin       # GPIO pins
from machine import ADC       # Analog-to-digital converter
from machine import PWM       # Pulse width modulation
from machine import I2C       # I2C communication
import time                    # Timing and delays
import network                 # WiFi (Pico W / 2W only)
```

## GPIO — Digital Output

```python
from machine import Pin

# Set up an output pin
led = Pin(15, Pin.OUT)

# Turn on / off
led.value(1)    # ON
led.value(0)    # OFF

# Toggle (flip the current state)
led.toggle()

# Onboard LED (Pico W / Pico 2W)
led = Pin("LED", Pin.OUT)
```

## GPIO — Digital Input

```python
from machine import Pin

# Button with internal pull-up resistor
button = Pin(14, Pin.IN, Pin.PULL_UP)

# Read the value (0 = pressed, 1 = not pressed with PULL_UP)
if button.value() == 0:
    print("Button pressed!")
```

## Analog Input (ADC)

```python
from machine import ADC

# Read from GP26 (ADC0)
sensor = ADC(26)

# Read a value (0 to 65535)
value = sensor.read_u16()
print(value)
```

## PWM

```python
from machine import Pin, PWM

# Set up PWM on a pin
pwm = PWM(Pin(15))
pwm.freq(1000)            # Frequency in Hz
pwm.duty_u16(32768)       # Duty cycle: 0 (off) to 65535 (full on)

# Common duty cycle values:
# 0     = 0% (off)
# 16384 = 25%
# 32768 = 50%
# 49152 = 75%
# 65535 = 100% (full on)
```

## Timing

```python
import time

time.sleep(1)       # Wait 1 second
time.sleep(0.5)     # Wait half a second
time.sleep_ms(100)  # Wait 100 milliseconds
time.sleep_us(50)   # Wait 50 microseconds

# Get current time in milliseconds (for measuring)
start = time.ticks_ms()
# ... do something ...
elapsed = time.ticks_diff(time.ticks_ms(), start)
print("That took", elapsed, "ms")
```

## WiFi (Pico 2W only)

```python
import network
import time

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("YourWiFiName", "YourPassword")

while not wlan.isconnected():
    print("Connecting...")
    time.sleep(1)

print("Connected!")
print("IP address:", wlan.ifconfig()[0])
```

## Useful Tips

- **Stop a running program:** Click the red Stop button in Thonny, or press Ctrl+C
- **Reset the Pico:** Unplug and replug USB
- **Save to Pico:** In Thonny, File > Save As > select "Raspberry Pi Pico"
- **Auto-run on power up:** Save your file as `main.py` on the Pico

---
*Circuit Explorers — MicroPython Quick Reference*
