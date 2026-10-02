# Twilight-Detector
An LED and a light sensor (photoresistor/LDR). When the room darkens, the light turns on automatically, and when daylight appears, it turns off.  17h logged · 4 entries
# Smart Automatic Nightlight

An automatic ambient-light-sensing nightlight created for **Hack Club's Half-Life** hardware program.

---

## 📌 Overview
This project uses a photoresistor (LDR) and a microcontroller to automatically turn an LED on when the room darkens and turn it off when daylight or ambient room light is detected.

* **Core Function:** Automatic light threshold detection using an Analog-to-Digital Converter (ADC).
* **Microcontroller:** Seeed Studio XIAO RP2040 (or any RP2040 / ESP32 / Arduino compatible board).
* **Language:** MicroPython.

---

## ⚙️ How It Works

1. **Light Sensing:** The photoresistor (LDR) is paired with a 10kΩ fixed resistor to form a **voltage divider**.
2. **Analog Read:** As ambient light drops, the LDR's resistance increases, raising the output voltage fed into analog pin `A0` (GPIO26).
3. **Software Logic:** The microcontroller samples the voltage via a 16-bit ADC reading (`0`–`65535`).
4. **Output Trigger:** If the sensor reading exceeds the software darkness threshold, pin `D1` outputs a `HIGH` signal (3.3V) to turn on the LED.

---

## 🔌 Circuit & Schematic Details

### Wiring Connections:
* **LDR Divider:**
  * Connect one leg of the **LDR** to `3V3`.
  * Connect the junction between the **LDR** and the **10kΩ resistor** to pin **`A0`**.
  * Connect the remaining leg of the **10kΩ resistor** to **`GND`**.
* **LED Control:**
  * Connect pin **`D1`** to a **220Ω resistor**.
  * Connect the resistor to the positive leg (Anode) of the **LED**.
  * Connect the negative leg (Cathode) of the **LED** to **`GND`**.

---

## 🛒 Bill of Materials (BOM)

| Component | Description | Qty | Approx. Price |
| :--- | :--- | :--- | :--- |
| **Microcontroller** | Seeed Studio XIAO RP2040 | 1 | ~$5.00 |
| **Light Sensor** | LDR Photoresistor (5528) | 1 | ~$0.20 |
| **Output LED** | 5mm Red or Warm White LED | 1 | ~$0.15 |
| **Resistor R1** | 10kΩ 1/4W | 1 | ~$0.05 |
| **Resistor R2** | 220Ω 1/4W | 1 | ~$0.05 |
| **Breadboard** | Half-size Solderless Breadboard | 1 | ~$1.50 |
| **Jumper Wires** | Male-to-Male Jumper Wires | 4 | ~$0.50 |
| **Power Cable** | USB-C Cable | 1 | ~$1.00 |
| **Total Estimated Budget** | | | **~$8.45** |

---

## 💻 MicroPython Code

Upload this `main.py` file to your board using Thonny IDE:

```python
import time
from machine import ADC, Pin

# Configure ADC on pin A0 (GPIO26 on RP2040) and digital output on D1 (GPIO1)
ldr = ADC(Pin(26))
led = Pin(1, Pin.OUT)

# Calibrated darkness threshold (0 - 65535)
DARK_THRESHOLD = 30000

while True:
    # Read 16-bit analog sensor value
    light_level = ldr.read_u16()

    if light_level >= DARK_THRESHOLD:
        led.value(1)  # Room is dark -> Turn ON LED
    else:
        led.value(0)  # Room is bright -> Turn OFF LED

    time.sleep(0.1)
