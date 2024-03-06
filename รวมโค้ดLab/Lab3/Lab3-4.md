## 4.ใช้ตัวต้านทานปรับค่าได้ เพิ่มลดความเร็วมอเตอร์และใช้ปุ่ม เพื่อเปลี่ยนการหมุนมอเตอร์เป็นทวนเข็มนาฬิกาและตามเข็มนาฬิกา
```ruby
#include <Arduino.h>

const int motorPin1 = D5; // สำหรับควบคุมทิศทางการหมุน
const int motorPin2 = D6; // สำหรับควบคุมทิศทางการหมุน
const int buttonPin = D1;
int motorSpeed ;

// กำหนดตัวแปรสำหรับ State Machine
enum State { STOPPED, clockwise, anticlockwise };
State currentState = clockwise; 


void setup() {
  Serial.begin(115200);
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);
  pinMode(A0, INPUT);
  pinMode(buttonPin, INPUT);

}

void loop() {

  switch (currentState) {

    case clockwise:
      motorSpeed =map(analogRead(A0),0,1023,0,255);
      Serial.println("clockwise  "+ String(motorSpeed));
      analogWrite(motorPin1, motorSpeed); 
      analogWrite(motorPin2, LOW);
      if (digitalRead(buttonPin) == HIGH) {
        while (digitalRead(buttonPin) == HIGH)
        {
        }
        currentState = anticlockwise; 
      }
      break;

    case anticlockwise:
      motorSpeed =map(analogRead(A0),0,1023,0,255);
      Serial.println("anticlockwise  "+ String(motorSpeed));
      analogWrite(motorPin1, LOW); 
      analogWrite(motorPin2, motorSpeed);
      if (digitalRead(buttonPin) == HIGH) {
        while (digitalRead(buttonPin) == HIGH)
        {
        }
        currentState = clockwise; 
      break;
  }
}
}
```