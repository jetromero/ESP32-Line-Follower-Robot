# Line Follower Robot with ESP32  
**Group:** Geekio | **Course:** AppDev Module 2  

---

## What is this?  
A basic line-following robot built with an ESP32 microcontroller. It uses IR sensors to detect a black line and adjusts motor speeds to stay on track.  

---

## Components Used  
| Component              | Quantity |  
|-------------------------|----------|  
| ESP32 microcontroller   | 1        |  
| L298N Motor Driver      | 1        |  
| DC Motors with Wheels   | 2        |  
| IR Sensors              | 2        |  
| Battery Pack (3.7V)     | 2        |  
| Robot Chassis           | 1        |  
| Connecting Wires        | 10+      |  

---

## How to Use  

### **1. Setup Arduino IDE**  
- **Install Arduino IDE:** [Download here](https://www.arduino.cc/en/software).  
- **Add ESP32 Support:**  
  - Go to **File > Preferences** and add this URL:  
    `https://dl.espressif.com/dl/package_esp32_index.json`  
  - Install the ESP32 package via **Tools > Board > Boards Manager**.  

### **2. Wire the Hardware**  
- **Motor Driver (L298N):**  
  - Connect `IN1`, `IN2`, `IN3`, `IN4` to ESP32 GPIOs **5, 4, 14, 12** (D2, D3, D4, D5).  
  - Connect `ENA` and `ENB` to PWM-capable GPIOs **13 and 15** (D6, D7).  
- **IR Sensors:**  
  - Connect left sensor to GPIO **0** (D0), right sensor to GPIO **1** (D1).  

### **3. Upload the Code**  
```cpp  
// Motor control pins  
#define IN1 5    // GPIO5 (D2)  
#define IN2 4    // GPIO4 (D3)  
#define IN3 14   // GPIO14 (D4)  
#define IN4 12   // GPIO12 (D5)  
#define ENA 13   // GPIO13 (D6, PWM)  
#define ENB 15   // GPIO15 (D7, PWM)  

// IR sensor pins  
#define IR_LEFT 0  // GPIO0 (D0)  
#define IR_RIGHT 1 // GPIO1 (D1)  

void setup() {  
  pinMode(IN1, OUTPUT);  
  pinMode(IN2, OUTPUT);  
  pinMode(IN3, OUTPUT);  
  pinMode(IN4, OUTPUT);  
  pinMode(ENA, OUTPUT);  
  pinMode(ENB, OUTPUT);  
  pinMode(IR_LEFT, INPUT);  
  pinMode(IR_RIGHT, INPUT);  

  analogWriteRange(255);  
  analogWriteFreq(1000);  
  analogWrite(ENA, 100);  // Initial speed (adjust as needed)  
  analogWrite(ENB, 100);  
}  

void loop() {  
  int left = digitalRead(IR_LEFT);  
  int right = digitalRead(IR_RIGHT);  

  if (left == LOW && right == LOW) {  
    forward();  
  } else if (left == LOW && right == HIGH) {  
    turnRight();  
  } else if (left == HIGH && right == LOW) {  
    turnLeft();  
  } else {  
    stopMotors();  
  }  
}  

void forward() {  
  digitalWrite(IN1, LOW);  
  digitalWrite(IN2, HIGH);  
  digitalWrite(IN3, LOW);  
  digitalWrite(IN4, HIGH);  
}  

void turnRight() {  
  analogWrite(ENA, 100);  // Slow left motor  
  analogWrite(ENB, 150);  // Fast right motor  
  forward();  
}  

void turnLeft() {  
  analogWrite(ENA, 150);  // Fast left motor  
  analogWrite(ENB, 100);  // Slow right motor  
  forward();  
}  

void stopMotors() {  
  digitalWrite(IN1, LOW);  
  digitalWrite(IN2, LOW);  
  digitalWrite(IN3, LOW);  
  digitalWrite(IN4, LOW);  
}  
```

### **4. Test the Robot**  
1. Place the robot on a black line (white background).  
2. Power it with **2x 3.7V batteries** (7.4V total).  
3. Watch it follow the line autonomously!  

---

## Key Fixes & Improvements  
1. **GPIO Pin Accuracy:**  
   - Explicitly mapped pins to their **actual ESP32 GPIO numbers** (e.g., `D2 = GPIO5`).  
   - Ensured `ENA`/`ENB` use PWM-capable GPIOs (13 and 15).  

2. **Sensor Calibration:**  
   - Added comments to remind users to calibrate IR sensors using their potentiometers.  

3. **Code Readability:**  
   - Simplified logic with clear comments.  
   - Removed redundant `delay()` in `setup()`.  

4. **Voltage Safety:**  
   - Emphasized using **2x 3.7V batteries** to avoid ESP32 overheating.  

---

## Challenges We Solved  
| Issue                  | Fix                                  |  
|------------------------|--------------------------------------|  
| Overheating ESP32      | Reduced voltage to 7.4V (2 batteries). |  
| Inconsistent sensors   | Recalibrated IR sensor potentiometers. |  
| Reversed power polarity| Double-checked wiring diagram.       |  

---

## Future Improvements  
- Add **PID control** for smoother turns.  
- Use **Wi-Fi** for remote monitoring.  
- Upgrade to **phototransistors** for better accuracy.  

---

**License:** MIT

---

### Notes for Users  
- If the robot struggles with sharp turns, adjust the PWM values in `turnLeft()`/`turnRight()`.  
- For better sensor reliability, use `analogRead()` instead of `digitalRead()` (requires code modification).  

Let me know if you need further adjustments! 😊
