## 1.แสดงค่าตัวต้านทานปรับค่าได้ผ่าน LCD
``` ruby
#include "Arduino.h"
#include "Wire.h"                   // เรียกใช้งาน Library Wire
#include "LiquidCrystal_I2C.h"      // เรียกใช้งาน Library LiquidCrystal_I2C
LiquidCrystal_I2C lcd(0x27, 16, 2); // ประกาศ Address กำหนดขนาดของหน้าจอ
const int DISPLAY0 = 0;
int state;
int val;
void setup()
{
  state = DISPLAY0;
  lcd.init();      // เรียกใช้งาน lcd
  lcd.backlight(); // เปิดใช้งาน Backlight
  pinMode(A0, INPUT);
}
void loop()
{
  val=map(analogRead(A0),0,1023,0,100);
  switch (state)
  {
  case DISPLAY0:
    lcd.setCursor(3, 0);      // กำหนดตำแหน่งตำแหน่งที่ 3 แถวที่ 0
    lcd.print("analogread"); // กำหนดข้อความที่จะแสดง
    lcd.setCursor(6, 1);      // กำหนดตำแหน่งตำแหน่งที่ 6 แถวที่ 1
    lcd.print(val);     // กำหนดข้อความที่จะแสดง
    delay(1000);              // หน่วงเวลา 1 วินาที
    lcd.clear();              // ล้างหน้าจอ
    Serial.println(val);    
    break;
  }
}
```


