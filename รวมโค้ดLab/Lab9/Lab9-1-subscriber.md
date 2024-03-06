## เพิ่ม-ลด ความสว่าง LED

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
#define MQTT_TOPIC "Dashboard/LED"
WiFiClient client;
PubSubClient mqtt(client);

const int CHECK_CONNECT = 0;
const int RECEIVE_MESSAGE = 1;
const int WAIT = 2;
const int MOTOR1 = 3;
const int MOTOR2 = 4;
int val = 0;
int state;
String substate= "Clockwise";


void callback(char *topic, byte *payload, unsigned int length)
{
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] : ");

  String message = "";
  for (int i = 0; i < (int)length; i++)
  { // ทำให้เป็น int เหมือนกัน
    message += (char)payload[i];
  } 
analogWrite(D0, message.toInt()); // กำหนดค่า PWM ให้กับ D0
  Serial.println(message);
}

void setup()
{
  state = CHECK_CONNECT;
  Serial.begin(115200);
  pinMode(D0, OUTPUT); // กำหนด pin D0 เป็น OUTPUT
  WiFi.mode(WIFI_STA);

  Serial.println(WIFI_STA_NAME);

  // Connect WiFi
  Serial.println("WIFI Connecting..");
  WiFi.begin(WIFI_STA_NAME, WIFI_STA_PASS);
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

  // Connect MQTT Broker
  mqtt.setServer(MQTT_SERVER, MQTT_PORT);
  mqtt.setCallback(callback);
}

void loop()
{
  switch (state)
  {
  case CHECK_CONNECT:
    if (!mqtt.connected())
    {
      Serial.print("Attempting MQTT connection…");
      if (mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD))
      {
        Serial.println("MQTT Connected.");
        state = RECEIVE_MESSAGE;
      }
      else
      {
        Serial.println("MQTT Fail Connected.");
        state = CHECK_CONNECT;
      }
    }
    break;

  case RECEIVE_MESSAGE:
    mqtt.connect(MQTT_NAME, MQTT_USERNAME, MQTT_PASSWORD);
    mqtt.subscribe(MQTT_TOPIC);
    mqtt.loop();
    state = RECEIVE_MESSAGE;
    break;
  }
}

```