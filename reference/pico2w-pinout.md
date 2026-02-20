# Raspberry Pi Pico 2W — Pin Reference

## Board Layout

```
                    ┌─────USB─────┐
          GP0  [ 1] │             │ [40]  VBUS
          GP1  [ 2] │             │ [39]  VSYS
          GND  [ 3] │             │ [38]  GND
          GP2  [ 4] │             │ [37]  3V3_EN
          GP3  [ 5] │             │ [36]  3V3 (OUT)
          GP4  [ 6] │             │ [35]  ADC_VREF
          GP5  [ 7] │             │ [34]  GP28 / ADC2
          GND  [ 8] │             │ [33]  GND
          GP6  [ 9] │             │ [32]  GP27 / ADC1
          GP7  [10] │             │ [31]  GP26 / ADC0
          GP8  [11] │             │ [30]  RUN
          GP9  [12] │             │ [29]  GP22
          GND  [13] │             │ [28]  GND
         GP10  [14] │             │ [27]  GP21
         GP11  [15] │             │ [26]  GP20
         GP12  [16] │             │ [25]  GP19
         GP13  [17] │             │ [24]  GP18
          GND  [18] │             │ [23]  GND
         GP14  [19] │             │ [22]  GP17
         GP15  [20] │   BOOTSEL   │ [21]  GP16
                    └─────────────┘
```

## Pin Types

| Type | Pins | What They Do |
|------|------|-------------|
| **GPIO** | GP0–GP28 | General purpose digital input/output |
| **ADC** | GP26 (ADC0), GP27 (ADC1), GP28 (ADC2) | Analog-to-digital converter (read analog sensors) |
| **GND** | Pins 3, 8, 13, 18, 23, 28, 33, 38 | Ground — the "return path" for electricity |
| **3V3** | Pin 36 | 3.3V power output |
| **VBUS** | Pin 40 | 5V from USB |
| **LED** | Onboard (use `"LED"` in code) | Built-in LED on the board |

## PWM Channels

All GP0–GP28 pins support PWM (Pulse Width Modulation) for controlling brightness, servo motors, and buzzer tones.

## I2C Buses

| Bus | SDA | SCL |
|-----|-----|-----|
| I2C0 | GP0, GP4, GP8, GP12, GP16, GP20 | GP1, GP5, GP9, GP13, GP17, GP21 |
| I2C1 | GP2, GP6, GP10, GP14, GP18, GP26 | GP3, GP7, GP11, GP15, GP19, GP27 |

## SPI Buses

| Bus | MOSI | MISO | SCK | CS |
|-----|------|------|-----|----|
| SPI0 | GP3, GP7, GP19 | GP0, GP4, GP16 | GP2, GP6, GP18 | GP1, GP5, GP17 |
| SPI1 | GP11, GP15 | GP8, GP12 | GP10, GP14 | GP9, GP13 |

## UART

| Bus | TX | RX |
|-----|----|----|
| UART0 | GP0, GP12, GP16 | GP1, GP13, GP17 |
| UART1 | GP4, GP8 | GP5, GP9 |

## Quick Code Reference

```python
from machine import Pin, ADC, PWM, I2C

# Digital output (LED, buzzer)
pin = Pin(15, Pin.OUT)
pin.value(1)

# Digital input (button)
button = Pin(14, Pin.IN, Pin.PULL_UP)
print(button.value())

# Analog input (sensor)
adc = ADC(26)
print(adc.read_u16())

# PWM (breathing LED, servo)
pwm = PWM(Pin(15))
pwm.freq(1000)
pwm.duty_u16(32768)

# I2C (OLED display)
i2c = I2C(0, sda=Pin(0), scl=Pin(1))

# Onboard LED
led = Pin("LED", Pin.OUT)
```

---
*Circuit Explorers — Pico 2W Pin Reference*
