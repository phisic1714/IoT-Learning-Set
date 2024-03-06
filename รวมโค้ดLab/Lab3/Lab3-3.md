## 3.	ใช้ตัวต้านทานปรับค่าได้ คุมทิศทางของมอเตอร์ (ถ้าค่าตัวต้านทาน0-124 มอเตอร์จะหมุนตามเข็ม และค่า 125-255 มอเตอร์จะหมุนทางซ้าย)
```ruby
#include <Arduino.h>

const int CLOCKWISE = 0; 
const int ANTI_CLOCKWISE = 1; 
const int motorPin1 = D5; 
const int motorPin2 = D6; 
int state ;

void setup() {
  
  Serial.begin(115200);  
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);
  pinMode(A0, INPUT); 
  state = CLOCKWISE;
}

void loop() {
  int potValue = analogRead(A0);
  int motorSpeed = map(potValue, 0, 1023, 0, 255);
  Serial.println(motorSpeed);

  switch (state) {
    case CLOCKWISE:
      analogWrite(motorPin1, 50);
      analogWrite(motorPin2, 0);
      if (motorSpeed >= 125) {
        state = ANTI_CLOCKWISE;
      }
      break;

    case ANTI_CLOCKWISE:
      analogWrite(motorPin1, 0);
      analogWrite(motorPin2, 50);
      if (motorSpeed < 125) {
        state = CLOCKWISE;
      }
      break;
  }
}
```