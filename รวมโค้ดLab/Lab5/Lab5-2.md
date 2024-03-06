### 2. บันทึกค่าอุณหภูมิและความชื้นลง SD Card
```ruby
#include <Arduino.h>
#include "SHTC3.h" 
#include "Wire.h" 
#include <SPI.h>
#include <SD.h>

SHTC3 shtc(Wire);

#define chipSelect D8 // Pin ที่เชื่อมกับ CS ของ SD Card Module
const int CHECK_DATA = 0;
const int RECORD_DATA = 1;
float temp, humid;
int state;

File dataFile;
bool shouldStartRecording();
void saveData();

void setup() {
  Serial.begin(115200);

  //กำหนด state เริ่มต้นทำงานแรก
  state = CHECK_DATA;
  //กำหนด Pin ของ SS เป็น OUTPUT
  pinMode(SS, OUTPUT);
  //กำหนดเริ่มต้นการทำงาน Wire และ SHTC
  Wire.begin();
  shtc.begin();
  //กำหนดเปิดไฟล์ "data.txt" ใน SD Card
  dataFile = SD.open("data.txt", FILE_WRITE);

  //ตรวจสอบ SD Card
  if (!SD.begin(chipSelect)) {
    //ไม่เจอ SD Card
    Serial.println("Card failed, or not present");
    return;
  }
    //เจอ SD Card
  Serial.println("Card initialized.");
}

void loop() {
  switch (state) 
  {
    case CHECK_DATA:  //ตรวจสอบมีค่าข้อมูล
      if (shouldStartRecording()) {
        state = RECORD_DATA;
      }
      break;
    case RECORD_DATA:
      saveData();
      state = CHECK_DATA;
      break;
  }
}

bool shouldStartRecording() {
   temp = shtc.readTempC();
  humid = shtc.readHumidity();

  // ตรวจสอบว่าไฟล์ "data.txt" มีอยู่หรือไม่
  if (!SD.exists("data.txt")) {
    Serial.println("File not found. Creating new file.");
    dataFile.close();
    return false;
  }
    return true;
}

void saveData() {
  //อ่านค่าอุณหภูมิและความชื้น
  temp = shtc.readTempC();
  humid = shtc.readHumidity();
  //เปิดไฟล์ "data.txt" แล้วเขียนข้อมูลลงในไฟล์
  dataFile = SD.open("data.txt", FILE_WRITE);
  //บันทึกข้อมูลอุณหภูมิและความชื้นในไฟล์ "data.txt" ใน SD Card
  if (dataFile) {
    dataFile.print("Temperature: ");
    dataFile.print(temp);
    dataFile.print(" °C\t");
    dataFile.print("Humidity: ");
    dataFile.print(humid);
    dataFile.println(" %");
    dataFile.close();
    //บันทึกข้อมูลสำเร็จ
    Serial.println("Data recorded");
  } else {
    //บันทึกข้อมูลไม่สำเร็จ
    Serial.println("Error opening file");
  }
}

```