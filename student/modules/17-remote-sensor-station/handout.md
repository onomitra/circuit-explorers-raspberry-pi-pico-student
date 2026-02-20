# Remote Sensor Station Cheat Sheet -- Module 17

## Wiring: Two Voltage Dividers

```
   Temperature Sensor (GP26):
   3.3V ──── Thermistor ──── junction A ──── GP26 (ADC0)
                                  |
                             [10k ohm]
                                  |
                                 GND

   Light Sensor (GP27):
   3.3V ──── Photoresistor ──── junction B ──── GP27 (ADC1)
                                    |
                               [10k ohm]
                                    |
                                   GND
```

Each junction must have the sensor, 10k resistor, and GPIO wire in the **same breadboard row**.

## Reading Both Sensors

```python
from machine import ADC
import math

temp_adc = ADC(26)                 # Thermistor on GP26
light_adc = ADC(27)               # Photoresistor on GP27

def read_temperature():
    val = temp_adc.read_u16()
    if val == 0: val = 1
    resistance = 10000 * val / (65535 - val)
    temp_k = 1 / (1/(273.15+25) + (1/3950) * math.log(resistance/10000))
    return temp_k - 273.15         # Returns Celsius

def read_light():
    val = light_adc.read_u16()
    return round(val / 65535 * 100)  # Returns 0-100%
```

## HTML Auto-Refresh

```html
<meta http-equiv="refresh" content="5">
```

Put this in the `<head>` section. The page reloads every 5 seconds automatically.

## Socket Serve Pattern

```python
import socket

addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]
s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind(addr)
s.listen(1)

while True:
    cl, client_addr = s.accept()       # Wait for browser
    request = cl.recv(1024).decode()   # Read request

    # Read sensors and build page here...

    cl.send("HTTP/1.1 200 OK\r\n")
    cl.send("Content-Type: text/html\r\n")
    cl.send("Connection: close\r\n\r\n")
    cl.send(html_string)               # Send page
    cl.close()                         # Done
```

## Server Flow

```
Browser (phone)               Pico (server)
    |                              |
    |--- GET / HTTP/1.1 --------->|
    |                              | reads thermistor + LDR
    |<-- HTTP/1.1 200 OK --------|
    |    <html>24.3 C, 72%</html>|
    |                              |
    | (5 seconds pass...)         |
    |--- GET / HTTP/1.1 --------->|  (auto-refresh!)
    |                              | reads sensors again
    |<-- fresh data! ------------|
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **IoT (Internet of Things)** | Physical devices connected to a network, sharing sensor data |
| **Auto-refresh** | HTML meta tag that reloads the page on a timer |
| **Sensor station** | A device that reads sensors and reports data remotely |
| **HTTP response** | The data a server sends back to a browser |
| **Dashboard** | A web page showing live data in an easy-to-read format |
| **Voltage divider** | Two resistors sharing voltage to read a variable sensor |

---
*Circuit Explorers -- Module 17*
