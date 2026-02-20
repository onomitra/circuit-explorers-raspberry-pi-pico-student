# LED Bar Cheat Sheet — Module 3

## Wiring Reference

```
  Pico Pin  →  Resistor (220Ω)  →  LED Bar Segment
  ─────────────────────────────────────────────────
  GP6       →  [220Ω]           →  Segment 1
  GP7       →  [220Ω]           →  Segment 2
  GP8       →  [220Ω]           →  Segment 3
  GP9       →  [220Ω]           →  Segment 4
  GP10      →  [220Ω]           →  Segment 5
  GP11      →  [220Ω]           →  Segment 6
  GP12      →  [220Ω]           →  Segment 7
  GP13      →  [220Ω]           →  Segment 8
  GP14      →  [220Ω]           →  Segment 9
  GP15      →  [220Ω]           →  Segment 10
  GND       →  (wire)           →  Common cathode (-)
```

## List Syntax

```python
# Create a list
pins = [6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

# Access by index (starts at 0!)
first = pins[0]     # 6
last = pins[9]      # 15

# Create a list by adding items one at a time
leds = []
leds.append(something)

# How many items?
count = len(leds)    # 10
```

## For Loop Patterns

```python
# Loop through a list
for item in my_list:
    print(item)

# Loop with numbers
for i in range(10):       # 0, 1, 2, ... 9
    print(i)

# Loop backwards
for i in range(9, -1, -1):  # 9, 8, 7, ... 0
    print(i)
```

## Vocabulary

| Word | Meaning |
|------|---------|
| **List** | A variable holding multiple items in order |
| **Index** | Position number in a list (starts at 0) |
| **for loop** | Repeats once for each item in a list or range |
| **range()** | Generates numbers — `range(10)` = 0 through 9 |
| **append()** | Adds an item to the end of a list |
| **Pattern** | A repeating sequence of actions |

---
*Circuit Explorers — Module 3*
