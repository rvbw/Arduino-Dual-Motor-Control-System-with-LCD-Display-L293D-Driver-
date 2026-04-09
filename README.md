# Arduino Dual Motor Control System with LCD Display

## Overview
This project demonstrates a simple embedded system using Arduino to control two DC motors through an L293D motor driver. The system performs different movement patterns such as forward, backward, left, and right, while displaying the current action on an I2C LCD screen.

The project is implemented using Tinkercad and is suitable for beginners learning motor control and communication protocols like I2C.

![Circuit](circuit.png)
---

## Components Used
- Arduino Uno
- L293D Motor Driver IC
- 2x DC Motors
- LCD 16x2 (I2C)
- Breadboard
- Jumper Wires
- 9V Battery (for motors)

---

## Circuit Connections

### L293D Connections
- IN1 → Arduino Pin 8  
- IN2 → Arduino Pin 9  
- IN3 → Arduino Pin 10  
- IN4 → Arduino Pin 11  

- Enable 1,2 → 5V  
- Enable 3,4 → 5V  

- Vcc1 (Power1) → 5V (Arduino)  
- Vcc2 (Power2) → 9V Battery  

- GND → Common Ground  

---

### Motors
- Motor A → Outputs 1 & 2  
- Motor B → Outputs 3 & 4  

---

### LCD (I2C)
- VCC → 5V  
- GND → GND  
- SDA → A4  
- SCL → A5  

> Note: Ensure the I2C address in the code matches the LCD settings in Tinkercad (e.g., 0x27).

---

## How It Works
The Arduino sends HIGH/LOW signals to the L293D driver to control motor direction:
- Forward
- Backward
- Right
- Left

At the same time, the LCD displays the current movement using the I2C communication protocol.

---

## Arduino Code
```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

int IN1 = 8;
int IN2 = 9;
int IN3 = 10;
int IN4 = 11;

void setup() {
  Wire.begin();
  
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  lcd.begin(16, 2);
  lcd.backlight();

void forward() {
  lcd.clear();
  lcd.print("Forward");

  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void backward() {
  lcd.clear();
  lcd.print("Backward");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void right() {
  lcd.clear();
  lcd.print("Right");

  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void left() {
  lcd.clear();
  lcd.print("Left");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopMotors() {
  lcd.clear();
  lcd.print("Stop");

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void loop() {
  forward();
  delay(30000);

  backward();
  delay(60000);

  for (int i = 0; i < 6; i++) {
    right();
    delay(5000);

    left();
    delay(5000);
  }

  stopMotors();
  while(1);
}
