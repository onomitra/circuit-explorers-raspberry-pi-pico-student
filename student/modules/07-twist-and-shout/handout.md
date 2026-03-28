# Potentiometer & ADC Cheat Sheet — Module 7

## Potentiometer Wiring

```
   3.3V ──── wire ──── Pot Left Pin
   GND  ──── wire ──── Pot Right Pin
   GP26 ──── wire ──── Pot Middle Pin (wiper)

   GP15 ──── [220 ohm] ──── LED (+) ──── LED (-) ──── GND
```

The potentiometer has 3 pins: power (left), ground (right), signal (middle).

## Reading the Potentiometer

```python
from machine import ADC

adc = ADC(26)                  # GP26 = ADC0
value = adc.read_u16()         # Returns 0 to 65535
```

| Knob Position | Approximate Value |
|---------------|-------------------|
| All the way left | ~65535 |
| Middle | ~32768 |
| All the way right | ~0 |

## Controlling LED Brightness with the Pot

```python
from machine import Pin, ADC, PWM

adc = ADC(26)
led_pwm = PWM(Pin(15))
led_pwm.freq(1000)

while True:
    pot_value = adc.read_u16()
    led_pwm.duty_u16(pot_value)   # 0-65535 matches perfectly!
```

## Mapping Values

```python
def map_range(value, in_min, in_max, out_min, out_max):
    return out_min + (value - in_min) * (out_max - out_min) // (in_max - in_min)

# Example: pot value (0-65535) to percentage (0-100)
percentage = map_range(pot_value, 0, 65535, 0, 100)
```

## ADC Pins on the Pico

Only 3 GPIO pins support ADC: **GP26** (ADC0), **GP27** (ADC1), **GP28** (ADC2).

## Vocabulary

| Word | Meaning |
|------|---------|
| **Analog** | A smooth, continuous signal (like a volume knob) |
| **Digital** | A signal with only distinct levels (like a light switch) |
| **ADC** | Analog-to-Digital Converter — turns voltage into a number |
| **Potentiometer** | A variable resistor with a twistable knob |
| **Range mapping** | Converting a number from one range to another |
| **Resolution** | How many levels the ADC can distinguish (16-bit = 65,536 levels) |
| **PWM** | Pulse Width Modulation — simulates in-between values |

---
*Circuit Explorers — Module 7*
