# robotcar
move straight
#include<LiquidCrystal.h> 

// LCD pins: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(8, 9, 4, 5, 6, 7);

// L298N motor driver pins
const int ENA = 3;  // Enable pin for right motor (PWM)
const int IN1 = 13;   // right motor direction 1
const int IN2 = 2;   // right motor direction 2
const int ENB = 11;   // Enable pin for left motor (PWM)
const int IN3 = 12;   // left motor direction 1
const int IN4 = 1;  // left motor direction 2

unsigned long startTime = 0;   // time when car starts moving
unsigned long currentTime = 0; // current time in seconds

void setup() {
  // LCD setup
  lcd.begin(16, 2);
  lcd.print("Robot Car Ready"); 
  
  delay(2000);
  lcd.clear();

  // Motor pins setup
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  // Start moving forward
  moveForward();
  startTime = millis();  // record start time
  delay(10000);
  stop();
}

void loop() {
  currentTime = (millis() - startTime) / 1000; // convert ms to seconds

  // Display on LCD
  lcd.setCursor(0, 0);
  lcd.print("Moving forward  ");
  lcd.setCursor(0, 1);
  lcd.print("Time: ");
  lcd.print(currentTime);
  lcd.print(" sec ");

  // Keep moving
  analogWrite(ENA, 140);  // 0–255 (speed control)
  analogWrite(ENB, 180);
}

// Move forward
void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 140);  // left motor speed
  analogWrite(ENB, 180);  // right motor speed
}

void stop() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
