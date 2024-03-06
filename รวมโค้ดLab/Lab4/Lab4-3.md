## 3.แสดงตัวเลข 4321 ผ่าน 7-segment ด้วย MCP23008
```ruby
#include <Arduino.h>
#include <Adafruit_MCP23X08.h> // ประกาศใช้งานไลบารี่ Adafruit_MCP23X08
Adafruit_MCP23X08 mcp;         // ประกาศตัวแปร mcp ให้เป็นตัวแปรของคลาส Adafruit_MCP23X08
int Digit_pins[] = {D5, D6, D7, D8}; // กำหนด Port สำหรับขา Digit 4 ถึง 1
int Segment_pin[] = {0,1,2,3,4,5,6}; // กำหนด Port สำหรับขา Segment  A ถึง G
const int SHOW_DIGIT_1 = 1;
const int SHOW_DIGIT_2 = 2;
const int SHOW_DIGIT_3 = 3;
const int SHOW_DIGIT_4 = 4;
int state;

void setup()
{
    mcp.begin_I2C(); 
    state = SHOW_DIGIT_1;
    for (int i = 0; i < 4; i++)
    {
        pinMode(Digit_pins[i], OUTPUT);
    }    
    for (int i = 0; i < 7; i++)
    {
        mcp.pinMode(Segment_pin[i], OUTPUT);
    }  
}

void loop()
{
    switch (state)
    {
    case SHOW_DIGIT_1: 
    
         // กำหนดให้ขาเปิดใช้ขา Digit 1 ทำงาน และปิดขา Digit 2,3,4
        digitalWrite(Digit_pins[0], HIGH);
        digitalWrite(Digit_pins[1], HIGH);
        digitalWrite(Digit_pins[2], HIGH);
        digitalWrite(Digit_pins[3], LOW);
        // แสดงเลข 4 บน 7 Segment
        mcp.digitalWrite(Segment_pin[0], LOW);
        mcp.digitalWrite(Segment_pin[1], HIGH);
        mcp.digitalWrite(Segment_pin[2], HIGH);
        mcp.digitalWrite(Segment_pin[3], LOW);
        mcp.digitalWrite(Segment_pin[4], LOW);
        mcp.digitalWrite(Segment_pin[5], HIGH);
        mcp.digitalWrite(Segment_pin[6], HIGH);
        delay(500);
        state = SHOW_DIGIT_2;        
        break;
    
   case SHOW_DIGIT_2:
    
        // กำหนดให้ขาเปิดใช้ขา Digit 2 ทำงาน และปิดขา Digit 1,3,4
        digitalWrite(Digit_pins[0], HIGH);
        digitalWrite(Digit_pins[1], HIGH);
        digitalWrite(Digit_pins[2], LOW);
        digitalWrite(Digit_pins[3], HIGH);
        // แสดงเลข 3 บน 7 Segment
        mcp.digitalWrite(Segment_pin[0], HIGH);
        mcp.digitalWrite(Segment_pin[1], HIGH);
        mcp.digitalWrite(Segment_pin[2], HIGH);
        mcp.digitalWrite(Segment_pin[3], HIGH);
        mcp.digitalWrite(Segment_pin[4], LOW);
        mcp.digitalWrite(Segment_pin[5], LOW);
        mcp.digitalWrite(Segment_pin[6], HIGH);
        delay(500);
        state = SHOW_DIGIT_3;
        break;
    
    case SHOW_DIGIT_3:
    
        // กำหนดให้ขาเปิดใช้ขา Digit 3 ทำงาน และปิดขา Digit 1,2,4
        digitalWrite(Digit_pins[0], HIGH);
        digitalWrite(Digit_pins[1], LOW);
        digitalWrite(Digit_pins[2], HIGH);
        digitalWrite(Digit_pins[3], HIGH);
        // แสดงเลข 2 บน 7 Segment
        mcp.digitalWrite(Segment_pin[0], HIGH);
        mcp.digitalWrite(Segment_pin[1], HIGH);
        mcp.digitalWrite(Segment_pin[2], LOW);
        mcp.digitalWrite(Segment_pin[3], HIGH);
        mcp.digitalWrite(Segment_pin[4], HIGH);
        mcp.digitalWrite(Segment_pin[5], LOW);
        mcp.digitalWrite(Segment_pin[6], HIGH);
        delay(500);
        state = SHOW_DIGIT_4;
    break;

   case SHOW_DIGIT_4:
    
         // กำหนดให้ขาเปิดใช้ขา Digit 4 ทำงาน และปิดขา Digit 1,2,3
        digitalWrite(Digit_pins[0], LOW);
        digitalWrite(Digit_pins[1], HIGH);
        digitalWrite(Digit_pins[2], HIGH);
        digitalWrite(Digit_pins[3], HIGH);
        // แสดงเลข 1 บน 7 Segment
        mcp.digitalWrite(Segment_pin[0], LOW);
        mcp.digitalWrite(Segment_pin[1], HIGH);
        mcp.digitalWrite(Segment_pin[2], HIGH);
        mcp.digitalWrite(Segment_pin[3], LOW);
        mcp.digitalWrite(Segment_pin[4], LOW);
        mcp.digitalWrite(Segment_pin[5], LOW);
        mcp.digitalWrite(Segment_pin[6], LOW);
        delay(500);
        state = SHOW_DIGIT_1;
        break;
    }
}
```