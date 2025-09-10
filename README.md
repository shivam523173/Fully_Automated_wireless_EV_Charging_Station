# 🚀 Ultrasonic Sensor Based Relay Control with LCD Display

This Arduino project uses **two ultrasonic sensors** to measure distances and control **two relays** accordingly.  
The system also displays real-time distance values and relay states on a **16x2 I2C LCD**.

---

## 📌 Features
- Two ultrasonic sensors for object detection.
- Two relays (active LOW type) for switching devices.
- LCD display for showing:
  - Sensor distances (in cm).
  - Relay ON/OFF status.
- Serial monitor output for debugging.
- Safe defaults (relays OFF at startup).
- Timeout handling for missed ultrasonic echoes.

---

## 🛠️ Hardware Required
- Arduino (Uno/Nano/MEGA or compatible).
- 2 × Ultrasonic sensors (HC-SR04).
- 2 × Relay modules (active LOW).
- 1 × 16x2 I2C LCD (address `0x27` or `0x3F`).
- Jumper wires & breadboard.

---

## ⚡ Pin Connections

| Component            | Arduino Pin |
|----------------------|-------------|
| **Ultrasonic Sensor 1 Trig** | 4 |
| **Ultrasonic Sensor 1 Echo** | 5 |
| **Ultrasonic Sensor 2 Trig** | 6 |
| **Ultrasonic Sensor 2 Echo** | 9 |
| **Relay 1**          | 7 |
| **Relay 2**          | 8 |
| **LCD I2C SDA**      | A4 |
| **LCD I2C SCL**      | A5 |

---

## 📜 How It Works
1. Each ultrasonic sensor continuously measures distance.
2. If a detected object is **within 10 cm**:
   - Corresponding relay is **activated (ON)**.
   - Status is updated on the LCD and Serial Monitor.
3. If no object is detected or distance ≥ 10 cm:
   - Relay remains **OFF**.
4. Both distance values and relay statuses are refreshed every second.

---


---

## ▶️ Getting Started
1. Install **Arduino IDE**.
2. Install required libraries:
   - `Wire.h` (built-in).
   - `LiquidCrystal_I2C.h`.
3. Upload the provided `.ino` code to your Arduino board.
4. Connect hardware as per the pinout table.
5. Open the Serial Monitor (`9600 baud`) to see live readings.

---

## 📷 System Overview
(Add your circuit diagram or real hardware photo here)

---

## ⚠️ Notes
- Make sure your relay modules are **active LOW** (this code assumes HIGH = OFF, LOW = ON).
- If your LCD does not display anything, try changing the I2C address from `0x27` to `0x3F`.

---

## 📄 License
This project is open-source and free to use under the **MIT License**.


## 🖥️ LCD Display Example
