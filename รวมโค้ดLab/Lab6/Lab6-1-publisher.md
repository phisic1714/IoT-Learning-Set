## กำหนดฝั่ง Publisher พิมพ์ข้อความ ON หรือ OFF ผ่าน Serial Monitor และส่งไปฝั่ง Subscriber รับข้อความเพื่อเปิด-ปิด LED
//Publisher
``` ruby
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

#define WIFI_STA_NAME "YOUR WIFI" //ชื่อ wifi
#define WIFI_STA_PASS "YOUR PASSWORD" //รหัส wifi
#define MQTT_SERVER "YOUR SERVER" //Server Domain Name หรือ IP Address
#define MQTT_PORT 1883 //Port MQTT Broker

//ข้อมูลผู้ใช้ ที่ใช้กับ Server
#define MQTT_USERNAME "YOUR USERNAME"
#define MQTT_PASSWORD "YOUR PASSWORD"
#define MQTT_NAME "YOUR NAME"

//ชื่อ Topic
#define MQTT_TOPIC "led_control"

WiFiClient client;
PubSubClient mqtt(client);

//กำหนด state ต่างๆ
const int CHECK_CONNECT = 0;
const int SEND_MESSAGE = 1;
int state;

void setup()
{
    pinMode(2, OUTPUT);
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
            state = SEND_MESSAGE;
        }else{
            Serial.println("MQTT Fail Connected.");
            state = CHECK_CONNECT;
        }
    
    break;

 case SEND_MESSAGE: 
    mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD);
    Serial.print("Input: ");
      while (!Serial.available()){} //เมื่อไม่มีการพิมพ์ใดๆ บน Serial monitor  ก็จะทำงานวนใน Loop นี้
  String input =Serial.readString(); //อ่านค่า String จากการพิมพ์ผ่านบน Serial monitor โดยเก็บค่าที่อ่านได้ไว้ในตัวแปรล input
Serial.println(input);
    if(input =="ON") //พิมพ์ ON ไฟติด
    {mqtt.publish(MQTT_TOPIC, "ON");
        Serial.println("Success, LED ON");
    }
    else if(input =="OFF")
    {mqtt.publish(MQTT_TOPIC, "OFF");
        Serial.println("Success, LED OFF");
    }else{
        Serial.println("Fail to sending");
    }
    break;
 }
}
```