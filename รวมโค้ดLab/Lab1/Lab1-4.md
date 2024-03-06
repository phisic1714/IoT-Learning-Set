## 4 แสดง LED 7 Segment ตามตำแหน่งที่กำหนด
### ตำแหน่ง A > F > G > C> D > E > G > B > A 
```ruby
#include "Arduino.h"
int Digit_pins[] = {D0, D1, D2, D3};
int Segment_pin[] = {D4, D5, D6, D7, D8, D9, D10};
const int SEGMENT_A_ROUND1 = 0;
const int SEGMENT_B = 1;
const int SEGMENT_G_ROUND1 = 2;
const int SEGMENT_E = 3;
const int SEGMENT_D = 4;
const int SEGMENT_C = 5;
const int SEGMENT_G_ROUND2 = 6;
const int SEGMENT_F = 7;
const int SEGMENT_A_ROUND2 = 8;
int state;
unsigned int Seg_val; // เก็บค่าที่จะส่งไปที่ Segment
void Segment_Display(int Seg_val); // ประกาศฟังก์ชันแสดงผล 7 Segment
void setup()
{
    state = SEGMENT_A_ROUND1;
    for (int i = 0; i < 4; i++)
    {
        pinMode(Digit_pins[i], OUTPUT);
    }

    for (int i = 0; i < 7; i++)
    {
        pinMode(Segment_pin[i], OUTPUT);
    }
}
void loop()
{
    switch (state)
    {
    case SEGMENT_A_ROUND1: // สถานะ SEGMENT_A
        Seg_val = 0x01; // เก็บค่าที่จะสั่งให้ทำงานแต่ล่ะ Segment ลงในตัวแปร Seg_val
        Segment_Display(Seg_val); // ส่งค่า Seg_val ไปที่ฟังก์ชัน Segment_Display
        state = SEGMENT_F; // เปลี่ยนสถานะไป SEGMENT_B
        break;
    case SEGMENT_F:
        Seg_val = 0x20;
        Segment_Display(Seg_val);
        state = SEGMENT_G_ROUND1;
        break;
    case SEGMENT_G_ROUND1:
        Seg_val = 0x40;
        Segment_Display(Seg_val);
        state = SEGMENT_C;
        break; 
    case SEGMENT_C:
        Seg_val = 0x04;
        Segment_Display(Seg_val);
        state = SEGMENT_D;
        break;
    case SEGMENT_D:
        Seg_val = 0x08;
        Segment_Display(Seg_val);
        state = SEGMENT_E;
        break;
    case SEGMENT_E:
        Seg_val = 0x10;
        Segment_Display(Seg_val);
        state = SEGMENT_G_ROUND2;
        break;    
    case SEGMENT_G_ROUND2:
        Seg_val = 0x40;
        Segment_Display(Seg_val);
        state = SEGMENT_B;
        break;   
    case SEGMENT_B:
        Seg_val = 0x02;
        Segment_Display(Seg_val);
        state = SEGMENT_A_ROUND2;
        break;
    case SEGMENT_A_ROUND2: // สถานะ SEGMENT_A
        Seg_val = 0x01; // เก็บค่าที่จะสั่งให้ทำงานแต่ล่ะ Segment ลงในตัวแปร Seg_val
        Segment_Display(Seg_val); // ส่งค่า Seg_val ไปที่ฟังก์ชัน Segment_Display
        state = SEGMENT_A_ROUND1; // เปลี่ยนสถานะไป SEGMENT_B
        break;
    }
}
void Segment_Display(int Seg_val) // ฟังก์ชันแสดงผล 7 Segment
{
    for (int i = 0; i <= 6; i++)
    {
        digitalWrite(Segment_pin[i], bitRead(Seg_val, i));
    }
    delay(500);
}
```