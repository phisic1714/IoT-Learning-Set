```ruby
#include <Arduino.h>
#include "SHTC3.h"
#include <Firebase_ESP_Client.h> // Firebase library
#include <ESP8266WiFi.h>
#include "addons/TokenHelper.h" // Firebase token generation
#include "addons/RTDBHelper.h"  // Firebase Realtime Database helper
#define WIFI_SSID "YOUR WIFI"
#define WIFI_PASSWORD "YOUR PASSWORD"
#define API_KEY "AIzaSyCGWi2Izt_pFKS81SyJaWyBXj-j91MhLgU"                                // API key ของ Firebase ที่ได้รับจากการสร้างโปรเจคใน Firebase Console
#define USER_EMAIL "phisic1714@gmail.com"                             // อีเมลของบัญชี Firebase ที่ใช้สำหรับเข้าสู่ระบบ
#define USER_PASSWORD "pee201100"                          // รหัสผ่านของบัญชี Firebase ที่ใช้สำหรับเข้าสู่ระบบ
#define DATABASE_URL "https://fir-lab-f4f74-default-rtdb.asia-southeast1.firebasedatabase.app/"                           // URL ของ Firebase Realtime Database
void readTempHumid();                              // ฟังก์ชันสำหรับอ่านค่าอุณหภูมิและความชื้นจากเซ็นเซอร์
void sendDataToFirebase(String path, float value); // ฟังก์ชันสำหรับส่งค่าอุณหภูมิและความชื้นไปยัง Firebase
FirebaseData fbdo;                                 // Object สำหรับเก็บข้อมูลที่ได้จาก Firebase
FirebaseAuth auth;                                 // Object สำหรับเก็บข้อมูลการเข้าสู่ระบบ
FirebaseConfig config;                             // Object สำหรับเก็บข้อมูลการเชื่อมต่อ Firebase
String uid;                                        // ตัวแปรสำหรับเก็บค่า User ID
String databasePath;                               // ตัวแปรสำหรับเก็บค่า path ของ Firebase Realtime Database
String tempPath;                                   // ตัวแปรสำหรับเก็บค่า path ของ Firebase Realtime Database สำหรับอุณหภูมิ
String humPath;                                    // ตัวแปรสำหรับเก็บค่า path ของ Firebase Realtime Database สำหรับความชื้น
SHTC3 shtc3(Wire);                                 
float temperature;                                
float humidity;                                   
const int READ_TEMP_HUMID = 0;
const int SEND_TEMP_HUMID = 1;
int state;
void initWiFi()
{
    WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
    Serial.print("Connecting to WiFi ..");
    while (WiFi.status() != WL_CONNECTED)
    {
        Serial.print('.');
        delay(1000);
    }
    Serial.println(WiFi.localIP());
    Serial.println();
}
void setup()
{
    state = READ_TEMP_HUMID;
    Serial.begin(115200);
    Wire.begin();
    initWiFi();
    config.api_key = API_KEY; // ตั้งค่า API key
    auth.user.email = USER_EMAIL; // ตั้งค่าอีเมลของบัญชี Firebase
    auth.user.password = USER_PASSWORD; // ตั้งค่ารหัสผ่านของบัญชี Firebase
    config.database_url = DATABASE_URL; // ตั้งค่า URL ของ Firebase Realtime Database
    Firebase.reconnectWiFi(true); // ตั้งค่าให้ Firebase ทำการเชื่อมต่อ WiFi ใหม่เมื่อเกิดการตัดสัญญาณ
    fbdo.setResponseSize(4096); // ตั้งค่าขนาดของข้อมูลที่ Firebase ส่งกลับมาให้เก็บไว้ใน Object
    config.token_status_callback = tokenStatusCallback; // ตั้งค่า callback function สำหรับการตรวจสอบสถานะของ Firebase token
    config.max_token_generation_retry = 5; // ตั้งค่าจำนวนครั้งที่ Firebase จะทำการสร้าง token ใหม่หากเกิดข้อผิดพลาด
    Firebase.begin(&config, &auth); // เริ่มต้นการทำงานของ Firebase
    Serial.println("Getting User UID");
    // ตรวจสอบสถานะของ Firebase token ว่ามีการสร้าง token แล้วหรือยัง
    while ((auth.token.uid) == "")
    {
        Serial.print('.');
        delay(1000);
    }
    databasePath = "Sensor/"; // ตั้งค่า path ของ Firebase Realtime Database
    tempPath = databasePath + "Temperature"; // ตั้งค่า path ของ Firebase Realtime Database สำหรับอุณหภูมิ
    humPath = databasePath + "Humidity";     // ตั้งค่า path ของ Firebase Realtime Database สำหรับความชื้น
}
void loop()
{
    switch (state)
    {
    case READ_TEMP_HUMID:
        readTempHumid();
        state = SEND_TEMP_HUMID;
        break;
    case SEND_TEMP_HUMID:
        sendDataToFirebase(tempPath, temperature); // ส่งค่าอุณหภูมิ และ path ไปยังฟังก์ชัน sendDataToFirebase
        sendDataToFirebase(humPath, humidity); // ส่งค่าความชื้น และ path ไปยังฟังก์ชัน sendDataToFirebase
        state = READ_TEMP_HUMID;
        break;
    }
}
void readTempHumid()
{
    shtc3.begin(true);
    shtc3.sample();
    // ถ้า Firebase พร้อมทำงาน จะเก็บค่าอุณหภูมิและความชื้น
    if (Firebase.ready())
    {
        delay(2000);
        temperature = shtc3.readTempC();
        humidity = shtc3.readHumidity();
    }
}
void sendDataToFirebase(String path, float value)
{
    // ถ้าส่งค่าไปยัง Firebase สำเร็จจะแสดงข้อความ "PASSED" และแสดง path และ type ของข้อมูลที่ Firebase ส่งกลับมา
    if (Firebase.RTDB.setFloat(&fbdo, path.c_str(), value))
    {
        Serial.print("Writing value: ");
        Serial.print(value);
        Serial.print(" on the following path: ");
        Serial.println(path);
        Serial.println("PASSED");
        Serial.println("PATH: " + fbdo.dataPath());
        Serial.println("TYPE: " + fbdo.dataType());
    }
    // ถ้าส่งค่าไปยัง Firebase ไม่สำเร็จจะแสดงข้อความ "FAILED" และแสดงเหตุผลที่เกิดข้อผิดพลาด
    else
    {
        Serial.println("FAILED");
        Serial.println("REASON: " + fbdo.errorReason());
    }
}
```
