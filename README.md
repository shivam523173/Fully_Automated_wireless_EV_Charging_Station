# 🚗⚡ Automated Wireless EV Charging Station

This project is a prototype for a **fully automated wireless EV charging station** using **Arduino, Ultrasonic Sensors, Relays, and an I2C LCD Display**.  
The system automatically detects when a car is in position and turns on the wireless charging module, eliminating the need for manual intervention.

---

## 🔧 Hardware Components
- **Arduino UNO / Nano**  
- **Ultrasonic Sensors (HC-SR04)** × 2 (for detecting car presence)  
- **Relay Modules (Active LOW)** × 2 (for switching charging circuits)  
- **16x2 I2C LCD Display** (for status monitoring)  
- **Wireless EV Charging Module (prototype coil-based setup)**  
- Jumper wires, power supply, breadboard  

---

## ⚙️ How It Works
1. **Car Detection**  
   - Two ultrasonic sensors are placed near the charging zone.  
   - They continuously measure the distance to detect whether a vehicle is in the correct charging spot.  

2. **Relay Control**  
   - If a car is detected within **10 cm** of a sensor, the corresponding relay is turned **ON**.  
   - This activates the **wireless charging pad**.  
   - When the car moves away, the relay is turned **OFF**, stopping the charging.  

3. **LCD Display & Serial Monitor**  
   - Shows live distance readings from both sensors.  
   - Displays relay status (`ON/OFF`) in real time.  
   - Helps in debugging and monitoring the system.  

---

## 🖥️ Arduino Code

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Define Ultrasonic Sensor 1 Pins
const int trig1 = 4;     
const int echo1 = 5;     

// Define Ultrasonic Sensor 2 Pins
const int trig2 = 6;     
const int echo2 = 9;     

// Define Relay Pins
const int relay1 = 7;    
const int relay2 = 8;    

// Initialize I2C LCD (Check address: 0x27 or 0x3F)
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  // Set pin modes for Ultrasonic Sensors
  pinMode(trig1, OUTPUT);
  pinMode(echo1, INPUT);
  pinMode(trig2, OUTPUT);
  pinMode(echo2, INPUT);

  // Set pin modes for Relays
  pinMode(relay1, OUTPUT);
  pinMode(relay2, OUTPUT);
  
  // Default state: Both relays OFF (HIGH for Active LOW relays)
  digitalWrite(relay1, HIGH);
  digitalWrite(relay2, HIGH);

  // Initialize Serial Monitor
  Serial.begin(9600);
  
  // Initialize LCD
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("System Ready...");
  delay(2000);
  lcd.clear();
}

// Function to measure distance
unsigned long getDistance(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  unsigned long duration = pulseIn(echoPin, HIGH, 30000); // Timeout after 30ms
  if (duration == 0) return 9999; // Return a large value if no echo is detected
  unsigned long distance = duration * 0.0343 / 2;
  return distance;
}

void loop() {
  // Get distances from both sensors
  unsigned long distance1 = getDistance(trig1, echo1);
  unsigned long distance2 = getDistance(trig2, echo2);

  Serial.print("D1: ");
  Serial.print(distance1);
  Serial.print(" cm | D2: ");
  Serial.print(distance2);
  Serial.println(" cm");

  // Display distances on LCD
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("D1:");
  lcd.print(distance1);
  lcd.print("cm D2:");
  lcd.print(distance2);
  lcd.print("cm");

  // Control Relay 1
  if (distance1 > 0 && distance1 < 10) {
    digitalWrite(relay1, LOW);  // Turn relay 1 ON
    Serial.println("Relay 1: ON ✅");
    lcd.setCursor(0, 1);
    lcd.print("R1: ON ");
  } else {
    digitalWrite(relay1, HIGH); // Turn relay 1 OFF
    Serial.println("Relay 1: OFF ❌");
    lcd.setCursor(0, 1);
    lcd.print("R1: OFF ");
  }

  // Control Relay 2
  if (distance2 > 0 && distance2 < 10) {
    digitalWrite(relay2, LOW);  // Turn relay 2 ON
    Serial.println("Relay 2: ON ✅");
    lcd.setCursor(8, 1);
    lcd.print("R2: ON ");
  } else {
    digitalWrite(relay2, HIGH); // Turn relay 2 OFF
    Serial.println("Relay 2: OFF ❌");
    lcd.setCursor(8, 1);
    lcd.print("R2: OFF");
  }

  delay(1000); // Delay for stability
}

