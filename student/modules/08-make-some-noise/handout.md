# Buzzer & Sound Cheat Sheet — Module 8

## Wiring: Buzzer + Button

```
  Buzzer:
  GP15 ──── wire ──── Buzzer (+)
                       Buzzer (-) ──── GND

  Button:
  GP14 ──── wire ──── Button (one side)
  GND  ──── wire ──── Button (other side)
```

**Tip:** The buzzer's (+) leg is longer, or marked with a `+` on top.

## PWM Buzzer Setup

```python
from machine import Pin, PWM

buzzer = PWM(Pin(15))
buzzer.freq(440)          # Set note frequency (440 = A4)
buzzer.duty_u16(32768)    # Turn on (50% duty = loudest)
buzzer.duty_u16(0)        # Turn off (silence)
```

## Note Frequency Chart (C Major Scale)

| Note | Frequency (Hz) |
|------|----------------|
| C4 | 262 |
| D4 | 294 |
| E4 | 330 |
| F4 | 349 |
| G4 | 392 |
| A4 | 440 |
| B4 | 494 |
| C5 | 523 |

Use `0` for a rest (silence).

## Playing a Note

```python
buzzer.freq(262)            # Set to C4
buzzer.duty_u16(32768)      # Sound on
time.sleep(0.4)             # Play for 0.4 seconds
buzzer.duty_u16(0)          # Sound off
time.sleep(0.1)             # Gap between notes
```

## Playing a Melody (Tuples)

```python
# Each tuple: (frequency, duration)
melody = [
    (262, 0.4),   # C4
    (294, 0.4),   # D4
    (330, 0.8),   # E4 (long)
    (0, 0.4),     # Rest (silence)
]

for freq, duration in melody:
    if freq == 0:
        buzzer.duty_u16(0)
    else:
        buzzer.freq(freq)
        buzzer.duty_u16(32768)
    time.sleep(duration)
    buzzer.duty_u16(0)
    time.sleep(0.05)
```

## Doorbell Pattern

```python
button = Pin(14, Pin.IN, Pin.PULL_UP)

while True:
    if button.value() == 0:       # Pressed
        buzzer.freq(800)
        buzzer.duty_u16(32768)    # Sound on
    else:
        buzzer.duty_u16(0)        # Sound off
    time.sleep(0.01)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **Frequency** | How many vibrations per second — measured in Hertz (Hz). Higher = higher pitch |
| **Hertz (Hz)** | The unit of frequency — 440 Hz means 440 vibrations per second |
| **Passive buzzer** | A buzzer with no built-in oscillator — you control the frequency to play any note |
| **Active buzzer** | A buzzer with a built-in oscillator — makes only one fixed tone |
| **Duty cycle** | For buzzers, 50% (32768) works best for volume |
| **Tuple** | A pair of values stored together, like `(440, 0.5)` for a note and its duration |
| **Melody** | A sequence of notes played in order with specific timing |

---
*Circuit Explorers — Module 8*
