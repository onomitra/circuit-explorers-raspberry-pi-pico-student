# WiFi Basics Cheat Sheet -- Module 15

## Connecting to WiFi

```python
import network
import time

wlan = network.WLAN(network.STA_IF)   # Station mode
wlan.active(True)                      # Turn on the radio

wlan.connect("NetworkName", "password")

while not wlan.isconnected():          # Wait for connection
    time.sleep(1)

print("IP:", wlan.ifconfig()[0])       # Print your IP address
```

## Scanning for Networks

```python
wlan = network.WLAN(network.STA_IF)
wlan.active(True)

networks = wlan.scan()
for net in networks:
    print(net[0].decode(), net[3])     # Name and signal strength
```

## Useful Methods

| Code | What It Does |
|------|--------------|
| `wlan.active(True)` | Turns the WiFi radio on |
| `wlan.active(False)` | Turns the WiFi radio off |
| `wlan.connect(SSID, PASSWORD)` | Connects to a network |
| `wlan.isconnected()` | Returns `True` if connected |
| `wlan.ifconfig()` | Returns (IP, subnet, gateway, DNS) |
| `wlan.status('rssi')` | Returns signal strength (negative number) |
| `wlan.scan()` | Lists nearby WiFi networks |
| `wlan.disconnect()` | Disconnects from the network |

## IP Address

A unique number for each device on the network:

```
  192 . 168 . 1 . 42
  --- . --- . - . --
  network part    device part
```

## Signal Strength (RSSI)

| RSSI (dBm) | Quality |
|-------------|---------|
| -30 to -50 | Excellent |
| -50 to -70 | Good |
| -70 to -80 | Fair |
| -80 to -90 | Weak |

Closer to 0 = stronger signal.

## Vocabulary

| Word | Meaning |
|------|---------|
| **WiFi** | Wireless network using radio waves |
| **SSID** | The name of a WiFi network |
| **IP Address** | A device's unique address on the network |
| **RSSI** | Signal strength measurement (in dBm) |
| **STA_IF** | Station mode -- connects to existing network |
| **Gateway** | The router -- your door to the internet |
| **network module** | MicroPython library for WiFi |

---
*Circuit Explorers -- Module 15*
