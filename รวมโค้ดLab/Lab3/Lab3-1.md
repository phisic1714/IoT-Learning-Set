## 1.การใช้ตัวต้านทานปรับค่าได้ ปรับความสว่างของ LED ภายนอก 
```ruby
#include <Arduino.h>
const int READ_ANALOG_INPUT = 0;
int state;
int Valmap;
void setup()
{
  state = 0;
  Serial.begin(115200);
  pinMode(D2, OUTPUT);
}
void loop()
{
  switch (state)
  {
  case READ_ANALOG_INPUT:
    int Val = analogRead(A0);
    Valmap = map(Val,0,1023,0,255);
    analogWrite(D2, Valmap);
    Serial.print("Val = ");
    Serial.println(Valmap);
    state = READ_ANALOG_INPUT;
  break;
  }
}
```