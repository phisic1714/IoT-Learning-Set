## 2.กดปุ่มไฟLEDดับกดอีกครั้งไฟLEDติดโดยใช้ Interrupt และDebounce ต่อวงจรแบบ Pull-Down
```ruby
#include <Arduino.h>

const int ledPin = D2;
const int buttonPin = D1;

// กำหนดตัวแปรสำหรับ State Machine
enum State { LED_OFF, LED_ON};
State currentState = LED_ON;

IRAM_ATTR void handleButtonPress() {
       while (digitalRead(buttonPin) == HIGH)
  if (currentState == LED_ON) {     
      currentState = LED_OFF;
    }    
    else if (currentState == LED_OFF)
    {
      currentState = LED_ON;
    }    
}

void setup() {
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);
  attachInterrupt(digitalPinToInterrupt(buttonPin), handleButtonPress, RISING);
}

void loop() {
  switch (currentState) {
    case LED_OFF:
    Serial.println("LED OFF");
      digitalWrite(ledPin, LOW);    
      break;
    case LED_ON:
      Serial.println("LED ON");
      digitalWrite(ledPin, HIGH);   
      break;
  }
}
```

