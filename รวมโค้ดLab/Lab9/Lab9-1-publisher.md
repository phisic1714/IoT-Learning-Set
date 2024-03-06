## เพิ่ม-ลด ความสว่าง LED

```ruby
[
    {
        "id": "7062dd1cd70ea125",
        "type": "tab",
        "label": "บท9",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "aa50f02b2ab36f95",
        "type": "mqtt out",
        "z": "7062dd1cd70ea125",
        "name": "",
        "topic": "Dashboard/LED",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "bc22f9fa38d5d08f",
        "x": 980,
        "y": 380,
        "wires": []
    },
    {
        "id": "932b5e6eca55e4d7",
        "type": "debug",
        "z": "7062dd1cd70ea125",
        "name": "debug 2",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 960,
        "y": 460,
        "wires": []
    },
    {
        "id": "b9df4e22d2b68417",
        "type": "ui_numeric",
        "z": "7062dd1cd70ea125",
        "name": "",
        "label": "Speed Value",
        "tooltip": "",
        "group": "f1fe76bfe30a6eb3",
        "order": 2,
        "width": 0,
        "height": 0,
        "wrap": false,
        "passthru": true,
        "topic": "topic",
        "topicType": "msg",
        "format": "{{value}}",
        "min": 0,
        "max": "255",
        "step": "15",
        "className": "",
        "x": 470,
        "y": 380,
        "wires": [
            [
                "932b5e6eca55e4d7",
                "aa50f02b2ab36f95"
            ]
        ]
    },
    {
        "id": "bc22f9fa38d5d08f",
        "type": "mqtt-broker",
        "name": "",
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
    },
    {
        "id": "f1fe76bfe30a6eb3",
        "type": "ui_group",
        "name": "Motor Control",
        "tab": "e419f86d6cd6cd21",
        "order": 5,
        "disp": true,
        "width": 5,
        "collapse": false,
        "className": ""
    },
    {
        "id": "e419f86d6cd6cd21",
        "type": "ui_tab",
        "name": "LED Control",
        "icon": "dashboard",
        "disabled": false,
        "hidden": false
    }
]
```
