# Thermometer Cheat Sheet -- Module 11

## Voltage Divider Wiring

```
   Temperature Sensor:
   3.3V ──── wire ──── Thermistor Pin 1
                        Thermistor Pin 2 ──── junction ──── wire ──── GP26 (ADC0)
                                                  |
                                             [10k ohm Resistor]
                                                  |
                                                 GND
```

The junction (where the thermistor, 10k resistor, and GP26 meet) must be in the **same breadboard row**.

## Reading the Raw Value

```python
from machine import ADC

adc = ADC(26)                      # GP26 = ADC0
raw_value = adc.read_u16()         # Returns 0 to 65535
```

| Condition | Approximate Reading |
|-----------|---------------------|
| Cold (thermistor cooled) | Low (~15000-25000) |
| Room temperature (~22 C) | Medium (~25000-35000) |
| Warm (finger touching) | High (~35000-45000) |

*Actual values depend on your thermistor and room temperature.*

## Temperature Conversion

```python
import math

adc_value = adc.read_u16()

# Step 1: ADC reading -> resistance
if adc_value == 0:
    adc_value = 1                          # Avoid divide-by-zero
resistance = 10000 * adc_value / (65535 - adc_value)

# Step 2: Resistance -> Kelvin (Steinhart-Hart)
temp_k = 1 / (1/(273.15 + 25) + (1/3950) * math.log(resistance/10000))

# Step 3: Kelvin -> Celsius -> Fahrenheit
temp_c = temp_k - 273.15
temp_f = temp_c * 9/5 + 32
```

## Print Formatting

```python
print("Temp:", round(temp_c, 1), "C /", round(temp_f, 1), "F")
```

`round(value, 1)` rounds to 1 decimal place.

## Conversion Quick Reference

| From | To | Formula |
|------|----|---------|
| Kelvin | Celsius | `temp_c = temp_k - 273.15` |
| Celsius | Fahrenheit | `temp_f = temp_c * 9/5 + 32` |
| Fahrenheit | Celsius | `temp_c = (temp_f - 32) * 5/9` |

## Data Logging Pattern

```python
readings = []                          # Empty list

for i in range(15):
    temp = read_temperature()
    readings.append(round(temp, 1))    # Add to list
    time.sleep(2)

print("Highest:", max(readings))
print("Lowest:", min(readings))
print("Average:", round(sum(readings) / len(readings), 1))
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **Thermistor** | A resistor whose resistance changes with temperature (thermal + resistor) |
| **NTC (Negative Temperature Coefficient)** | As temperature goes UP, resistance goes DOWN |
| **Steinhart-Hart Equation** | A formula that converts thermistor resistance to temperature |
| **Data logging** | Recording sensor readings over time for later analysis |
| **Celsius** | Temperature scale where water freezes at 0 and boils at 100 |
| **Fahrenheit** | Temperature scale where water freezes at 32 and boils at 212 |
| **Kelvin** | Temperature scale starting at absolute zero (0 K = -273.15 C) |
| **Voltage Divider** | Two resistors sharing voltage; the junction voltage changes when one resistor changes |

---
*Circuit Explorers -- Module 11*
