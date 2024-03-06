## 2.LED 2 ดวง ติด-ดับ สลับกัน
```ruby
#include <Arduino.h>

const int LED1_ON = 0;
const int LED2_ON = 1;
int state ;

void setup() {  
  Serial.begin(115200);
  pinMode(D0, OUTPUT);
  pinMode(D1, OUTPUT);
  digitalWrite(D0, LOW);
  digitalWrite(D1, LOW);  
}

void loop() {
  switch (state) {
    case LED1_ON:
      digitalWrite(D0, HIGH);
      digitalWrite(D1, LOW);      
      delay(1000); // หน่วงเวลา 1 วินาที
      state = LED2_ON;
      break;
    case LED2_ON:
      digitalWrite(D0, LOW);
      digitalWrite(D1, HIGH);      
      delay(1000); // หน่วงเวลา 1 วินาที
      state = LED1_ON;
      break;
  }
}
```

