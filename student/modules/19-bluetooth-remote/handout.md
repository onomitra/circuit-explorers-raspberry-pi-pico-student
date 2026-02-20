# Bluetooth Remote Cheat Sheet -- Module 19

## BLE Concepts

```
   Step 1: ADVERTISE          Step 2: CONNECT          Step 3: COMMUNICATE

   Pico: "I'm here!           Phone: "I see you!"      Phone sends: "on"
         My name is                                     Phone sends: "off"
         PicoRemote!"          Phone <---> Pico         Phone sends: "beep"
         ))))                  Connected!
         ))))                                           Pico acts on commands!
```

| Concept | What It Means |
|---------|---------------|
| **Advertising** | Pico broadcasts "I'm here!" so phones can find it |
| **Connecting** | Phone pairs with the Pico to exchange data |
| **Service** | A category of BLE data (like a menu section) |
| **Characteristic** | A specific piece of data within a service (like a menu item) |
| **UART** | The text-based service we use to send/receive strings |

## Phone Apps

| Platform | App Name | How to Connect |
|----------|----------|----------------|
| **Android** | Serial Bluetooth Terminal (free) | Menu -> Devices -> Bluetooth LE tab -> SCAN -> tap "PicoRemote" |
| **iOS** | LightBlue (free) | Auto-scans -> tap "PicoRemote" -> find UUID ending in 0002 -> Write new value |

## Command Reference

| Command | What It Does |
|---------|-------------|
| `on` | Turns the LED on |
| `off` | Turns the LED off |
| `beep` | Plays a short buzzer tone |
| `blink` | Blinks the LED 3 times |

## Wiring

```
   Pico 2W
   +-----------------------------+
   |                             |
   |  GP15 ──── [220 ohm] ──── LED (+) ──── LED (-) ──── GND
   |                             |
   |  GP14 ──── Buzzer (+) ──── Buzzer (-) ──── GND
   |                             |
   +-----------------------------+
```

No extra Bluetooth hardware needed -- the Pico 2W has BLE built in!

## Key Code Patterns

### Creating the BLE Device
```python
remote = BLERemote()          # Starts advertising
```

### Checking for Connection
```python
while not remote.is_connected():
    time.sleep(0.5)
```

### Reading Commands
```python
command = remote.read()       # Returns text or None
if command:
    command = command.lower()  # Make it lowercase
```

### Sending Text Back to Phone
```python
remote.send("LED is ON!")
```

### Command Parsing Pattern
```python
if command == "on":
    led.value(1)
elif command == "off":
    led.value(0)
elif command == "beep":
    do_beep()
else:
    print("Unknown:", command)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **BLE** | Bluetooth Low Energy -- a power-efficient version of Bluetooth |
| **Advertising** | Broadcasting "I'm here!" so other devices can find you |
| **UART** | Universal Asynchronous Receiver/Transmitter -- a standard for sending text over BLE |
| **Service** | A category of BLE data (like a section on a restaurant menu) |
| **Characteristic** | A specific data item within a service (like an item on the menu) |
| **Command parsing** | Reading text input and deciding what action to take |
| **Bluetooth** | Wireless technology for direct device-to-device communication |
| **Peripheral** | The BLE device that advertises and waits for connections (your Pico) |

---
*Circuit Explorers -- Module 19*
