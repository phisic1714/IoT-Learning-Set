## กดปุ่ม Inject Node เพื่อสส่งข้อความ INCREASE หรือ DECREASE ไปยัง subscriber

```ruby

[
    {
        "id": "a03704ad5a5d45c8",
        "type": "tab",
        "label": "Flow 2",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "193c0eb3262a7dc5",
        "type": "inject",
        "z": "a03704ad5a5d45c8",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "INCREASE",
        "payloadType": "str",
        "x": 310,
        "y": 180,
        "wires": [
            [
                "80668a04efa1ec83",
                "4eabb4f986459e79"
            ]
        ]
    },
    {
        "id": "05fee6552b43f977",
        "type": "inject",
        "z": "a03704ad5a5d45c8",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "DECREASE",
        "payloadType": "str",
        "x": 310,
        "y": 240,
        "wires": [
            [
                "80668a04efa1ec83",
                "4eabb4f986459e79"
            ]
        ]
    },
    {
        "id": "80668a04efa1ec83",
        "type": "mqtt out",
        "z": "a03704ad5a5d45c8",
        "name": "",
        "topic": "TestMQTT2",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "6ae0e7d3d862746a",
        "x": 630,
        "y": 240,
        "wires": []
    },
    {
        "id": "4eabb4f986459e79",
        "type": "debug",
        "z": "a03704ad5a5d45c8",
        "name": "debug 2",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 620,
        "y": 320,
        "wires": []
    },
    {
        "id": "6ae0e7d3d862746a",
        "type": "mqtt-broker",
        "name": "MQTT",
        "broker": "203.158.245.180",
        "port": "1883",
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "4",
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    }
]
```