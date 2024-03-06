## 1.กดปุ่มไฟLEDดับ ปล่อยปุ่มไฟLEDติด ต่อแบบ Pull-Down โดยใช้ Interrupt และ Debounce
```ruby
#include <Arduino.h>

const int ledPin = D2;
const int buttonPin = D1;

// กำหนดตัวแปรสำหรับ State Machine
enum State { LED_OFF, LED_ON};
State currentState = LED_ON;


IRAM_ATTR void ButtonPress() {
  if (digitalRead(buttonPin)==HIGH) {
    delay(100);
    Serial.println("Button Pressed");
    currentState = LED_OFF;
    }    
}

void setup() {  
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);
  attachInterrupt(digitalPinToInterrupt(buttonPin), ButtonPress, RISING);
}
void loop() {
  switch (currentState) {
    case LED_OFF:
    Serial.println("LED OFF");
      digitalWrite(ledPin, LOW);
      if(digitalRead(buttonPin)==LOW){
       currentState = LED_ON;   
      }         
      break;
    case LED_ON:
      Serial.println("LED ON");
      digitalWrite(ledPin, HIGH);     
      break;
  }
}
```


