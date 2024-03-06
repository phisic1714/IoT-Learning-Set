## 2.กดปุ่มที่1เพิ่มความเร็วมอเตอร์ และกดปุ่มที่2 ลดความเร็วมอเตอร์ 

```ruby
#include <Arduino.h>
const int SPEED_UP= 0;
const int SPEED_DOWN= 1;
const int SPEED = 2;
int state;

int motorSpeed = 0;  // ความเร็วเริ่มต้นของมอเตอร์

void setup() {
  Serial.begin(115200);
  state = SPEED;
  
  pinMode(D5, OUTPUT);
  pinMode(D6, OUTPUT);
  pinMode(D1, INPUT);
  pinMode(D2, INPUT);
}

void loop() {
  switch (state) {
    
    case SPEED:
      Serial.println("ความเร็วปัจจุบัน: " + String(motorSpeed));
      if (digitalRead(D1) == 1) {
        state = SPEED_UP;
      } else if (digitalRead(D2) == 1) {
        state = SPEED_DOWN;
      }
      analogWrite(D5, motorSpeed);
      analogWrite(D6, 0);
      break; 

    case SPEED_UP:
      if (motorSpeed < 250) {
        motorSpeed = motorSpeed + 50;  
      }
      else if (motorSpeed >= 250){
        motorSpeed = 250; 
      }     
      
      Serial.println("เพิ่มความเร็ว: " + String(motorSpeed));
      state = SPEED;
      delay(500);
      break;

    case SPEED_DOWN:
      if (motorSpeed > 50) {
        motorSpeed = motorSpeed-50;  
      }
      else if (motorSpeed <= 50){
        motorSpeed = 0; 
      }
      Serial.println("ลดความเร็ว: " + String(motorSpeed));
      state = SPEED;
      delay(500);
      break;

  }
}
```