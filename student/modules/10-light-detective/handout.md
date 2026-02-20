# Light Detective Cheat Sheet — Module 10

## Voltage Divider Wiring

```
   Light Sensor:
   3.3V ──── wire ──── Photoresistor Pin 1
                        Photoresistor Pin 2 ──── junction ──── wire ──── GP26 (ADC0)
                                                    |
                                               [10k ohm Resistor]
                                                    |
                                                   GND

   LED:
   GP15 ──── [220 ohm] ──── LED (+) ──── LED (-) ──── GND
```

The junction (where the LDR, 10k resistor, and GP26 meet) must be in the **same breadboard row**.

## Reading the Light Level

```python
from machine import ADC

adc = ADC(26)                      # GP26 = ADC0
light_value = adc.read_u16()       # Returns 0 to 65535
```

| Condition | Approximate Reading |
|-----------|---------------------|
| Bright light (flashlight) | High (~50000-65000) |
| Normal room light | Medium (~25000-45000) |
| Covered / dark | Low (~1000-15000) |

*Actual values depend on your LDR and room lighting.*

## Automatic Night Light (On/Off)

```python
from machine import Pin, ADC

adc = ADC(26)
led = Pin(15, Pin.OUT)
THRESHOLD = 25000                  # Adjust for YOUR room!

while True:
    light_value = adc.read_u16()
    if light_value < THRESHOLD:    # Dark
        led.value(1)               # LED ON
    else:                          # Bright
        led.value(0)               # LED OFF
```

## Brightness Control (PWM)

```python
from machine import Pin, ADC, PWM

adc = ADC(26)
led_pwm = PWM(Pin(15))
led_pwm.freq(1000)

while True:
    light_value = adc.read_u16()
    brightness = 65535 - light_value   # Invert: dark = bright LED
    led_pwm.duty_u16(brightness)
```

**Inversion trick:** `65535 - light_value` flips the scale so darkness produces high brightness.

## Vocabulary

| Word | Meaning |
|------|---------|
| **Photoresistor (LDR)** | Light Dependent Resistor — resistance changes with light |
| **Voltage Divider** | Two resistors sharing voltage; the junction voltage changes when one resistor changes |
| **Threshold** | A cutoff value for making decisions (e.g., "dark" vs "bright") |
| **ADC** | Analog-to-Digital Converter — turns voltage into a number (0-65535) |
| **Resistance** | How much a material resists electricity flow (measured in ohms) |
| **Inverting** | Flipping a value (`65535 - value`) so low becomes high and vice versa |
| **Sensor** | A component that detects something in the physical world |

---
*Circuit Explorers — Module 10*
