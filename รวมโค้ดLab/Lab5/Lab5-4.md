### 4. จำลองการเปิด-ปิดประตูเลื่อน โดยการแตะบัตร RFID ที่กำหนดเพื่อสั่งให้ DC motor หมุน เป็นเวลา 5 วินาที

```ruby

#include <Arduino.h>
#include <SD.h>
#include <SPI.h>
#include <MFRC522.h>
#include <Wire.h>

#define RST_PIN D1
#define SS_PIN D2 //RFID
#define MOTOR_PIN D0 // ขาที่เชื่อมต่อกับ DC motor

String rfid_in; 
String rfid_pass="54EA7AC"; //เก็บค่า RFID สามารถเปลี่ยนแปลงได้
String readRFID();
void openDoor(); // เพิ่มฟังก์ชันสำหรับเปิดประตู

const int READ_RFID = 0;
const int CHECK_RFID = 1;
const int OPEN_DOOR = 2; // เปลี่ยน WRITE_SD เป็น OPEN_DOOR

int state = CHECK_RFID;
MFRC522 mfrc522(SS_PIN, RST_PIN); // รุ่น rfid

void setup() 
{
    Serial.begin(115200);
    SPI.begin();
    mfrc522.PCD_Init();

    pinMode(MOTOR_PIN, OUTPUT);
}

void loop() 
{
    switch(state) 
    {    
        case READ_RFID: 
                rfid_in = readRFID();
                Serial.println("Read RFID");
                Serial.println("HEX: "+rfid_in);
                delay(1000);
                if(rfid_pass==rfid_in){
                    state = OPEN_DOOR; // เปลี่ยน state เมื่ออ่าน RFID สำเร็จ
                }
                else{
                    state = CHECK_RFID;
                }
            break;
        case CHECK_RFID:
            if (mfrc522.PICC_IsNewCardPresent() && mfrc522.PICC_ReadCardSerial()) {
                state = READ_RFID;
            } else {
                Serial.println("Wait RFID");
                delay(1000);
            }
            break;
            
        case OPEN_DOOR: // เปลี่ยนชื่อ state เป็น OPEN_DOOR
            openDoor(); // เรียกใช้ฟังก์ชันเพื่อเปิดประตู
            state = CHECK_RFID; // หลังจากเปิดประตูเสร็จ กลับไปที่ CHECK_RFID
            break;
    }
}

String readRFID()
{
    String content;
    for(byte i =0; i<mfrc522.uid.size; i++)
    {
        content.concat(String(mfrc522.uid.uidByte[i] <0x10?"0":""));
        content.concat(String(mfrc522.uid.uidByte[i], HEX));
    }
    content.toUpperCase();
    return content.substring(1);
}

void openDoor() {
    digitalWrite(MOTOR_PIN, HIGH);
    delay(5000); // ให้ DC motor หมุนเป็นเวลา 5 วินาที
    digitalWrite(MOTOR_PIN, LOW);
}

```