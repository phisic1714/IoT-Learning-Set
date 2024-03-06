## Node RED รับค่ามาแสดงบน Debug node 

```ruby
[
    {
        "id": "18803d926e227e23",
        "type": "mqtt in",
        "z": "862dae4c7b22e1e3",
        "name": "",
        "topic": "TestMQTT",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "6ae0e7d3d862746a",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 340,
        "y": 220,
        "wires": [
            [
                "78492ae8fb5dfd9b"
            ]
        ]
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