## พิมพ์ข้อความที่ต้องการผ่าน Serial monitor ส่งไปยัง Node RED 

```ruby
#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#define WIFI_STA_NAME "YOUR WIFI" //ชื่อ wifi
#define WIFI_STA_PASS "YOUR PASSWORD" //รหัส wifi
#define MQTT_SERVER "YOUR SERVER" //Server Domain Name หรือ IP Address
#define MQTT_PORT 1883 //Port MQTT Broker

// ข้อมูลผู้ใช้ ที่ใช้กับ Server
#define MQTT_USERNAME "admincpe"
#define MQTT_PASSWORD "cpe2541"
#define MQTT_NAME ""

// ชื่อ Topic
#define MQTT_TOPIC "TestMQTT"
WiFiClient client;
PubSubClient mqtt(client);
String data;
// กำหนด state ต่างๆ
const int CHECK_CONNECT = 0;
const int READ_DATA = 1;
const int SEND_DATA = 2;
int state;

void setup()
{

    state = CHECK_CONNECT;
    Serial.begin(115200);

    WiFi.mode(WIFI_STA);                      // กำหนดเป็นโหมด Station (WIFI_STA)
    WiFi.begin(WIFI_STA_NAME, WIFI_STA_PASS); // กำหนดค่าเริ่มต้น WiFi

    // Connect WiFi
    Serial.println("Connecting to WiFi..");
    for (int i = 0; i < 20; i++)
    {
        Serial.print(".");
        delay(500);
    }
    if (WiFi.status() == WL_CONNECTED)
    {
        Serial.println("Connected to WiFi");
    }
    else
    {
        Serial.println("Fail to Connected WiFi");
    }
    mqtt.setServer(MQTT_SERVER, MQTT_PORT);
    // Connect MQTT Broker
}

void loop()
{
    switch (state)
    {
    case CHECK_CONNECT: // check MQTT connected

        Serial.print("Attempting MQTT connection...");
        if (mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD))
        {
            Serial.println("MQTT Connected.");
            state = READ_DATA;
        }
        else
        {
            Serial.println("MQTT Fail Connected.");
            state = CHECK_CONNECT;
        }

        break;

    case READ_DATA:
        Serial.println("Enter Data: ");
        while (!Serial.available())
            ;
        data = Serial.readString();
        state = SEND_DATA;
        break;

    case SEND_DATA:
        if (mqtt.publish(MQTT_TOPIC, data.c_str()))
        {
            Serial.println("Success sending, " + data);
        }
        else
        {
            Serial.println("Fail sending, " + data);
        }
        state = READ_DATA;
        break;
    }
}
```