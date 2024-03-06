## 1.เปิด-ปิด LED ภายนอกโดยผ่าน Serial Monitor
```ruby

#include <Arduino.h>

const int LED_ON = 0;
const int LED_OFF = 1;
int state ;

void setup() {
  state=LED_OFF;  
  Serial.begin(115200);  
  pinMode(D2, OUTPUT);
}

void loop() {
  // ตรวจสอบ Serial Monitor
  while (!Serial.available()){} //เมื่อไม่มีการพิมพ์ใดๆ บน Serial monitor จะทำงานวนใน Loop นี้
  char command =Serial.read(); //อ่านค่าจากการพิมพ์ผ่าน Serial monitor โดยเก็บค่าที่อ่านได้ไว้ในตัวแปร command
  Serial.println(command); 
  switch (command) {
    case '1':    
      if (state == LED_OFF) {
        state = LED_ON;
        digitalWrite(D2, HIGH);
        Serial.println("เปิด LED");
      }
      break;
    case '0':
      if (state == LED_ON) {
        state = LED_OFF;
        digitalWrite(D2, LOW);
        Serial.println("ปิด LED");
      }
      break;
    default:
      Serial.println("คำสั่งไม่ถูกต้อง");  }
}
```


