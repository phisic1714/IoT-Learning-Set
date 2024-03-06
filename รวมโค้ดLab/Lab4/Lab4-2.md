## 2.LED ไฟวิ่งจำนวน 5 ดวง ด้วย MCP23008
```ruby
#include <Arduino.h>
#include <Adafruit_MCP23X08.h> // ประกาศใช้งานไลบารี่ Adafruit_MCP23X08
Adafruit_MCP23X08 mcp;         // ประกาศตัวแปร mcp ให้เป็นตัวแปรของคลาส Adafruit_MCP23X08
const int LED1 = 0;
const int LED2 = 1;
const int LED3 = 2;
const int LED4 = 3;
const int LED5 = 4;
int state;
void setup()
{
    state = LED1;
    mcp.begin_I2C(); // กำหนดใช้งานโปรโตคอล I2C
    mcp.pinMode(0, OUTPUT);    
    mcp.pinMode(1, OUTPUT); 
    mcp.pinMode(2, OUTPUT);
    mcp.pinMode(3, OUTPUT);
    mcp.pinMode(4, OUTPUT);
}
void loop()
{
    switch (state)
    {
    case LED1:
        mcp.digitalWrite(4, LOW);
        mcp.digitalWrite(0, HIGH);       
        delay(1000);
        state = LED2;
        break;
    case LED2:
        mcp.digitalWrite(0, LOW);
        mcp.digitalWrite(1, HIGH);     
        delay(1000);
        state = LED3;
        break;
      case LED3:
        mcp.digitalWrite(1, LOW);
        mcp.digitalWrite(2, HIGH);       
        delay(1000);
        state = LED4;
        break;
      case LED4:
        mcp.digitalWrite(2, LOW);
        mcp.digitalWrite(3, HIGH);        
        delay(1000);
        state = LED5;
        break;
      case LED5:
        mcp.digitalWrite(3, LOW);
        mcp.digitalWrite(4, HIGH);        
        delay(1000);
        state = LED1;
        break;
    }
}
```

