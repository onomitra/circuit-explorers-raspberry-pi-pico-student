# Ultrasonic Distance Sensor Cheat Sheet — Module 13

## HC-SR04 Wiring

```
   HC-SR04 Sensor:
   Pico VBUS (5V) ──── wire ──── HC-SR04 VCC
   Pico GND       ──── wire ──── HC-SR04 GND
   Pico GP15      ──── wire ──── HC-SR04 Trig
   Pico GP14      ──── wire ──── HC-SR04 Echo

   Buzzer:
   Pico GP13      ──── wire ──── Buzzer (+) ──── Buzzer (-) ──── GND
```

**Important:** The HC-SR04 needs 5V power — use VBUS, not 3.3V!

## Distance Measurement Code

```python
from machine import Pin, time_pulse_us
import time

trigger = Pin(15, Pin.OUT)
echo = Pin(14, Pin.IN)

def measure_distance():
    trigger.value(0)
    time.sleep_us(2)
    trigger.value(1)
    time.sleep_us(10)
    trigger.value(0)
    duration = time_pulse_us(echo, 1, 30000)
    if duration < 0:
        return -1
    return duration * 0.0343 / 2

while True:
    dist = measure_distance()
    print("Distance:", round(dist, 1), "cm")
    time.sleep(0.3)
```

## The Distance Formula

```
distance (cm) = echo time (us) x 0.0343 / 2
```

- **0.0343** = speed of sound in cm per microsecond (343 m/s)
- **Divide by 2** = sound travels TO the object AND back (round trip)

| Echo Time (us) | Calculation | Distance |
|---------------:|-------------|----------|
| 580 | 580 x 0.0343 / 2 | ~10 cm |
| 1166 | 1166 x 0.0343 / 2 | ~20 cm |
| 5831 | 5831 x 0.0343 / 2 | ~100 cm |

## Trigger Sequence

1. Set Trig LOW — wait 2 us
2. Set Trig HIGH — wait 10 us
3. Set Trig LOW
4. Sensor sends 8 ultrasonic pulses at 40 kHz
5. Echo pin goes HIGH until echo returns
6. `time_pulse_us(echo, 1, 30000)` measures the HIGH time

## Sensor Range

- **Minimum:** ~2 cm
- **Maximum:** ~400 cm (4 meters)
- **Accuracy:** +/- 3 mm
- Works best with flat surfaces aimed straight at the sensor

## Vocabulary

| Word | Meaning |
|------|---------|
| **Ultrasonic** | Sound above 20 kHz — too high for humans to hear |
| **HC-SR04** | Ultrasonic distance sensor with transmitter and receiver |
| **Trigger** | A short pulse that starts the measurement |
| **Echo** | The reflected sound signal that returns to the sensor |
| **time_pulse_us** | MicroPython function to time how long a pin stays HIGH |
| **Speed of sound** | ~343 m/s in air at room temperature |
| **Round-trip** | Sound goes to the object and back — divide by 2 for one-way distance |

---
*Circuit Explorers — Module 13*
