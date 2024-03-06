### 3. บันทึกค่า RFID ลงบน SD Card
```ruby
#include <Arduino.h>
#include <SD.h>
#include <SPI.h>
#include <MFRC522.h>
#include <Wire.h>

//กำหนด Pin ที่ต้องการใช้
#define RST_PIN D1
#define SS_PIN D2 //RFID
#define chipSelect D8 //SD Card

String rfid_in;  
String readRFID();
void writeSD();

//กำหนด state 
const int CHECK_RFID = 0;
const int CHECK_SD = 1;
const int READ_RFID = 2;
const int WRITE_SD = 3;

//กำหนด state แรกที่ทำงาน
int state = CHECK_RFID;
MFRC522 mfrc522(SS_PIN, RST_PIN); //รุ่น rfid

void setup() 
{
    Serial.begin(115200);
    SPI.begin();
    mfrc522.PCD_Init();

}
void loop() 
{
    switch(state) 
    {
        //เช็คการเชื่อมต่อ RFID
        case CHECK_RFID:
        if (mfrc522.PICC_IsNewCardPresent() && mfrc522.PICC_ReadCardSerial()) {
            state = READ_RFID;
        } else {
            Serial.println("Fail RFID");
        delay(1000);
        }
            state = READ_RFID;
            break;
        
        //อ่านค่า RFID
        case READ_RFID: 
        if(mfrc522.PICC_IsNewCardPresent()&&mfrc522.PICC_ReadCardSerial()){
            rfid_in = readRFID();
            Serial.println("Read RFID");
            Serial.println("HEX: "+rfid_in);
            delay(1000);
            state = CHECK_SD;
        }            
            break;
        //เช็คการเชื่อมต่อกับ SD Card
        case CHECK_SD: 
            if(SD.begin(chipSelect)) {
                Serial.println("Card initialized.");
                state = WRITE_SD;
            } else {
                Serial.println("Card failed");
                delay(10000); // รอ 10 วินาที
                state = CHECK_RFID;
            }
            break;
        //เขียนค่าลง SD Card
        case WRITE_SD:
            writeSD();
            state = CHECK_RFID;
            break;
    }
}

//อ่านค่า RFID แปลงจาก Byte เป็น HEX แล้วแปลงเป็นตัวหนังสือ
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

//เขียนข้อมูลลงไฟล์ "data.txt" ใน SD Card 
void writeSD(){
    File dataFile = SD.open("data.txt", FILE_WRITE);

    if(dataFile)
    {
        dataFile.print("RFID Value : ");
        dataFile.println(rfid_in);
        dataFile.close();
        //บันทึกข้อมูลสำเร็จ
        Serial.println("Data recorded");
    } else{
        //บันทึกข้อมูลไม่สำเร็จ
        Serial.println("Error opening file");
    }
}
```