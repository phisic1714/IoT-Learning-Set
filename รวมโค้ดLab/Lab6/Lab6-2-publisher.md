```ruby

#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include <SPI.h>
#include <Wire.h>
#include <MFRC522.h>
#define RST_PIN D1
#define SS_PIN D2

#define WIFI_STA_NAME "YOUR WIFI" //ชื่อ wifi
#define WIFI_STA_PASS "YOUR PASSWORD" //รหัส wifi
#define MQTT_SERVER "YOUR SERVER" //Server Domain Name หรือ IP Address
#define MQTT_PORT 1883 //Port MQTT Broker

//ข้อมูลผู้ใช้ ที่ใช้กับ Server
#define MQTT_USERNAME "admincpe"
#define MQTT_PASSWORD "cpe2541"
#define MQTT_NAME ""

//ชื่อ Topic
#define MQTT_TOPIC "rfid_control"
WiFiClient client;
PubSubClient mqtt(client);

//กำหนด state ต่างๆ
const int CHECK_CONNECT = 0;
const int READ_RFID = 1;
const int SEND_MESSAGE = 2;
MFRC522 mfrc522(SS_PIN, RST_PIN);

int state;
String initRFID = "54EA7AC";
String valRFID;
String readRFID();

void setup()
{
    state = READ_RFID;
    SPI.begin();
    mfrc522.PCD_Init();
    //pinMode(2, OUTPUT);
    state = CHECK_CONNECT;
    Serial.begin(115200);

    WiFi.mode(WIFI_STA); //กำหนดเป็นโหมด Station (WIFI_STA)
    WiFi.begin(WIFI_STA_NAME, WIFI_STA_PASS); //กำหนดค่าเริ่มต้น WiFi

    //Connect WiFi
    Serial.println("Connecting to WiFi..");
    for(int i = 0; i<20; i++){
        Serial.print(".");
        delay(500);
    }
    if(WiFi.status() == WL_CONNECTED) {
        Serial.println("Connected to WiFi");
    }else{
        Serial.println("Fail to Connected WiFi");
    }

    //Connect MQTT Broker
    mqtt.setServer(MQTT_SERVER, MQTT_PORT); 
}

void loop()
{
 switch (state)
 {
 case CHECK_CONNECT: //check MQTT connected
    
        Serial.print("Attempting MQTT connection...");
        if(mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD))
        {
            Serial.println("MQTT Connected.");
            state = READ_RFID;
        }else{
            Serial.println("MQTT Fail Connected.");
            state = CHECK_CONNECT;
        }
    
    break;
 case READ_RFID: //อ่านค่า RFID
       if(mfrc522.PICC_IsNewCardPresent()&&mfrc522.PICC_ReadCardSerial())
       {
          valRFID = readRFID();
          Serial.println("==========");
          Serial.println("Read RFID");
          //Serial.println("HEX: "+valRFID);
          Serial.println("==========");
          delay(1000);
          state = SEND_MESSAGE;
        }
        else{
        Serial.println("Wait RFID");
        delay(1000);
        }
        break;

 case SEND_MESSAGE: 
    mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD);
    if(valRFID == initRFID ) //พิมพ์ ON ไฟติด
    {mqtt.publish(MQTT_TOPIC, "ON");
        Serial.println("Success, LED ON");
        delay(10000);
    }
    else if(valRFID != initRFID )
    {// mqtt.publish(MQTT_TOPIC, "OFF");
        Serial.println("Access Denied");
    }else{
        Serial.println("Fail to sending");
    }
    state = READ_RFID;
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

```