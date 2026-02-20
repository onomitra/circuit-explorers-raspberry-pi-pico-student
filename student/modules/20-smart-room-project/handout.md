# Smart Room Cheat Sheet -- Module 20

## Complete Wiring Reference

```
   Photoresistor (light sensor) -- Voltage Divider on GP27:
   Pico 3.3V ──── Photoresistor ──── junction ──── GP27 (ADC1)
                                        |
                                   [10k ohm]
                                        |
                                      GND

   Thermistor (temperature sensor) -- Voltage Divider on GP26:
   Pico 3.3V ──── Thermistor ──── junction ──── GP26 (ADC0)
                                      |
                                 [10k ohm]
                                      |
                                    GND

   PIR Sensor (motion detector):
   Pico VBUS (5V) ──── PIR VCC
   Pico GND       ──── PIR GND
   Pico GP14      ──── PIR OUT    (check pin labels on YOUR sensor!)

   LED (night light + alarm):
   Pico GP15 ──── [220 ohm] ──── LED (+) ──── LED (-) ──── GND

   Buzzer (alarm sound):
   Pico GP13 ──── Buzzer (+) ──── Buzzer (-) ──── GND

   Button (arm/disarm):
   Pico GP12 ──── Button ──── GND   (use PULL_UP in code)
```

## Pin Summary

| Component | Pin | Type |
|-----------|-----|------|
| Photoresistor | GP27 (ADC1) | Analog input |
| Thermistor | GP26 (ADC0) | Analog input |
| PIR sensor | GP14 | Digital input |
| Button | GP12 | Digital input (PULL_UP) |
| LED | GP15 | Digital output |
| Buzzer | GP13 | PWM output |

## Function Checklist

Your Smart Room code needs these functions:

| Function | What It Does | Inputs |
|----------|-------------|--------|
| `read_sensors()` | Reads all sensors, returns a dictionary | None |
| `check_night_light(light)` | Turns LED on/off based on light level | Light percentage (0-100) |
| `check_temperature(temp)` | Prints warning if too hot | Temperature in Celsius |
| `check_motion(motion)` | Triggers alarm if armed and motion detected | PIR value (0 or 1) |
| `check_button()` | Toggles alarm armed/disarmed | None |
| `main()` | Main loop calling all functions | None |

## Key Code Patterns

### Reading light as a percentage
```python
adc_light = ADC(27)
light_raw = adc_light.read_u16()
light_pct = int(light_raw / 65535 * 100)
```

### Reading temperature in Celsius
```python
adc_temp = ADC(26)
val = adc_temp.read_u16()
if val == 0: val = 1
resistance = 10000 * val / (65535 - val)
temp_k = 1 / (1/(273.15+25) + (1/3950) * math.log(resistance/10000))
temp_c = round(temp_k - 273.15, 1)
```

### Reading PIR motion
```python
pir = Pin(14, Pin.IN)
if pir.value() == 1:
    print("Motion!")
```

### Buzzer on/off
```python
buzzer = PWM(Pin(13))
buzzer.freq(2000)
buzzer.duty_u16(5000)    # ON
buzzer.duty_u16(0)       # OFF
```

## Testing Checklist

Work through this list in order:

- [ ] Photoresistor reads different values in light vs dark
- [ ] Thermistor shows reasonable room temperature (~20-25 C)
- [ ] PIR detects motion after warm-up (wait 30-60 seconds!)
- [ ] LED turns on and off
- [ ] Buzzer sounds and stops
- [ ] Button press is detected
- [ ] Night light subsystem works (LED responds to light changes)
- [ ] Temperature warnings print when threshold is exceeded
- [ ] Motion alarm triggers when armed and motion detected
- [ ] All subsystems work together in the main loop
- [ ] Button arms and disarms the motion alarm

## Vocabulary

| Word | Meaning |
|------|---------|
| **Integration** | Combining separate parts into one working system |
| **Subsystem** | A smaller part of a bigger system (e.g., night light) |
| **Threshold** | A boundary value that triggers an action |
| **State machine** | A system that behaves differently depending on its mode |
| **IoT** | Internet of Things -- smart objects that sense and react |

---
*Circuit Explorers -- Module 20: Smart Room Project*
