## เพิ่มลด-ความเร็วมอเตอร์
```ruby
[
    {
        "id": "4a426243c3e7999a",
        "type": "tab",
        "label": "บท9",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "b4b18a341af27183",
        "type": "debug",
        "z": "4a426243c3e7999a",
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
        "id": "1597b7228354614c",
        "type": "ui_button",
        "z": "4a426243c3e7999a",
        "name": "",
        "group": "33ee3bd8ae7334b8",
        "order": 2,
        "width": 0,
        "height": 0,
        "passthru": false,
        "label": "Countr-Clockwise",
        "tooltip": "",
        "color": "",
        "bgcolor": "",
        "className": "",
        "icon": "",
        "payload": "Countr-Clockwise",
        "payloadType": "str",
        "topic": "topic",
        "topicType": "msg",
        "x": 572.8000335693359,
        "y": 528.0499877929688,
        "wires": [
            [
                "60775c2dbc172f95"
            ]
        ]
    },
    {
        "id": "0b73a163be93d48d",
        "type": "ui_button",
        "z": "4a426243c3e7999a",
        "name": "",
        "group": "33ee3bd8ae7334b8",
        "order": 1,
        "width": 0,
        "height": 0,
        "passthru": false,
        "label": "Clockwise",
        "tooltip": "",
        "color": "",
        "bgcolor": "",
        "className": "",
        "icon": "",
        "payload": "Clockwise",
        "payloadType": "str",
        "topic": "topic",
        "topicType": "msg",
        "x": 535.8000335693359,
        "y": 451.04998779296875,
        "wires": [
            [
                "60775c2dbc172f95"
            ]
        ]
    },
    {
        "id": "60775c2dbc172f95",
        "type": "mqtt out",
        "z": "4a426243c3e7999a",
        "name": "",
        "topic": "Dashboard/LED",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "7851de6d0dc36306",
        "x": 980,
        "y": 380,
        "wires": []
    },
    {
        "id": "d78c7aeb9bcbd4e1",
        "type": "ui_slider",
        "z": "4a426243c3e7999a",
        "name": "",
        "label": "slider",
        "tooltip": "",
        "group": "33ee3bd8ae7334b8",
        "order": 2,
        "width": 0,
        "height": 0,
        "passthru": true,
        "outs": "all",
        "topic": "topic",
        "topicType": "msg",
        "min": "0",
        "max": "150",
        "step": "30",
        "className": "",
        "x": 559.1999893188477,
        "y": 377.3249855041504,
        "wires": [
            [
                "60775c2dbc172f95"
            ]
        ]
    },
    {
        "id": "33ee3bd8ae7334b8",
        "type": "ui_group",
        "name": "Motor Control",
        "tab": "aeb368afa6b73636",
        "order": 5,
        "disp": true,
        "width": 5,
        "collapse": false,
        "className": ""
    },
    {
        "id": "7851de6d0dc36306",
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
        "id": "aeb368afa6b73636",
        "type": "ui_tab",
        "name": "LED Control",
        "icon": "dashboard",
        "disabled": false,
        "hidden": false
    }
]

```