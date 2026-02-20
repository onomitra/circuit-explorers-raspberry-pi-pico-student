# Web Dashboard Cheat Sheet -- Module 16

## WiFi + Socket Setup

```python
import network, socket, time
from machine import Pin

led = Pin(15, Pin.OUT)

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("SSID", "PASSWORD")
while not wlan.isconnected():
    time.sleep(1)
ip = wlan.ifconfig()[0]

addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]
s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind(addr)
s.listen(1)
```

## Handling a Request

```python
cl, client_addr = s.accept()         # Wait for connection
request = cl.recv(1024).decode()     # Read the request

if "/on" in request:
    led.value(1)
elif "/off" in request:
    led.value(0)

cl.send("HTTP/1.1 200 OK\r\n")
cl.send("Content-Type: text/html\r\n")
cl.send("Connection: close\r\n\r\n")
cl.send(html_string)
cl.close()
```

## Minimal HTML Page

```html
<!DOCTYPE html>
<html>
<head><title>My Pico</title></head>
<body>
  <h1>LED Control</h1>
  <a href="/on">Turn ON</a>
  <a href="/off">Turn OFF</a>
</body>
</html>
```

## Socket Steps

| Step | Code | What It Does |
|------|------|-------------|
| Create | `s = socket.socket()` | Opens a new phone line |
| Bind | `s.bind(addr)` | Assigns a phone number (address + port) |
| Listen | `s.listen(1)` | Starts waiting for calls |
| Accept | `s.accept()` | Picks up when someone calls |
| Receive | `cl.recv(1024)` | Listens to what the caller says |
| Send | `cl.send(data)` | Talks back to the caller |
| Close | `cl.close()` | Hangs up the phone |

## How HTTP Works

```
Browser (phone)               Pico (server)
    |                              |
    |--- GET /on HTTP/1.1 -------->|  (request)
    |                              |  LED turns on
    |<-- HTTP/1.1 200 OK ---------|  (response)
    |    <html>...</html>          |
    |                              |
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **HTTP** | The language browsers and servers speak |
| **Web server** | A program that sends web pages to browsers |
| **HTML** | The language web pages are written in |
| **Socket** | A connection endpoint (like a phone line) |
| **Request** | Browser asks server for a page |
| **Response** | Server sends back the page |
| **Port 80** | Standard door number for HTTP |
| **URL** | The address in the browser (e.g., http://192.168.1.42/on) |

---
*Circuit Explorers -- Module 16*
