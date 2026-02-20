# Robot Arm Basics — Module 9

## Wiring

### Servo (SG90)

| Servo Wire | Pico Pin |
|------------|----------|
| Brown (GND) | GND |
| Red (Power) | **VBUS (5V)** — NOT 3.3V! |
| Orange (Signal) | GP15 |

### Potentiometer

| Pot Pin | Pico Pin |
|---------|----------|
| Left | 3.3V |
| Middle | GP26 (ADC0) |
| Right | GND |

```
   Servo:   GP15 ──► Orange    VBUS ──► Red    GND ──► Brown
   Pot:     3.3V ──► Left      GP26 ──► Middle  GND ──► Right
```

## Key Code

```python
from machine import Pin, PWM, ADC

# Set up servo
servo = PWM(Pin(15))
servo.freq(50)           # Servos need 50 Hz

# Angle-to-duty function
def set_angle(angle):
    min_duty = 1640      # 0 degrees
    max_duty = 8200      # 180 degrees
    duty = min_duty + (max_duty - min_duty) * angle / 180
    servo.duty_u16(int(duty))

# Move to an angle
set_angle(90)

# Read potentiometer
pot = ADC(Pin(26))
value = pot.read_u16()   # Returns 0-65535

# Map pot to angle
angle = value * 180 / 65535
```

## Servo Angles and Duty Values

| Angle | Duty Value | Pulse Width |
|-------|-----------|-------------|
| 0 deg | 1640 | 1.0 ms |
| 90 deg | 4920 | 1.5 ms |
| 180 deg | 8200 | 2.0 ms |

## Vocabulary

| Word | Meaning |
|------|---------|
| **Servo motor** | Motor that moves to a specific angle and holds |
| **Pulse width** | Length of the ON signal — controls the angle |
| **VBUS** | Pico pin providing 5V from USB |
| **Mapping** | Converting a value from one range to another |
| **Function** | Reusable block of code with a name |

---
*Circuit Explorers — Module 9*
