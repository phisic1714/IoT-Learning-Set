## จากตัวอย่าง 8.1 ให้เพิ่ม Text node แสดงค่าอุณหภูมิ และความชื้น

```ruby


[
    {
        "id": "dc457c6f1b613044",
        "type": "tab",
        "label": "บท8",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "02f7ed67b7d8c44c",
        "type": "ui_chart",
        "z": "dc457c6f1b613044",
        "name": "",
        "group": "c551eaa1ae24df22",
        "order": 1,
        "width": 0,
        "height": 0,
        "label": "Temperature Graph",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "linear",
        "nodata": "",
        "dot": false,
        "ymin": "",
        "ymax": "",
        "removeOlder": 1,
        "removeOlderPoints": "",
        "removeOlderUnit": "60",
        "cutout": 0,
        "useOneColor": false,
        "useUTC": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "outputs": 1,
        "useDifferentColor": false,
        "className": "",
        "x": 978.0000076293945,
        "y": 616.9999694824219,
        "wires": [
            []
        ]
    },
    {
        "id": "0f7e9d2e83a35e4f",
        "type": "ui_chart",
        "z": "dc457c6f1b613044",
        "name": "",
        "group": "11a858d05f83445d",
        "order": 1,
        "width": 0,
        "height": 0,
        "label": "Humidity Graph",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "linear",
        "nodata": "",
        "dot": false,
        "ymin": "",
        "ymax": "",
        "removeOlder": 1,
        "removeOlderPoints": "",
        "removeOlderUnit": "60",
        "cutout": 0,
        "useOneColor": false,
        "useUTC": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "outputs": 1,
        "useDifferentColor": false,
        "className": "",
        "x": 968.0000076293945,
        "y": 796.9999694824219,
        "wires": [
            []
        ]
    },
    {
        "id": "661ea4c041f1c5bb",
        "type": "firebase-in",
        "z": "dc457c6f1b613044",
        "name": "",
        "constraint": {},
        "database": "acc072ae6bd90de3",
        "listenerType": "value",
        "outputType": "string",
        "path": "/Sensor/Humidity",
        "useConstraint": false,
        "x": 668.0000076293945,
        "y": 836.9999694824219,
        "wires": [
            [
                "0f7e9d2e83a35e4f",
                "e42253c097a73b8f"
            ]
        ]
    },
    {
        "id": "50ddc515eef199a3",
        "type": "firebase-in",
        "z": "dc457c6f1b613044",
        "name": "",
        "constraint": {},
        "database": "acc072ae6bd90de3",
        "listenerType": "value",
        "outputType": "auto",
        "path": "/Sensor/Temperature",
        "useConstraint": false,
        "x": 688.0000305175781,
        "y": 649.9999771118164,
        "wires": [
            [
                "02f7ed67b7d8c44c",
                "2c049dcc3a184ca6"
            ]
        ]
    },
    {
        "id": "fd308ac13081a7e0",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "ดึงค่าอุณหภูมิจาก Firebase",
        "info": "",
        "x": 698.0000076293945,
        "y": 616.9999694824219,
        "wires": []
    },
    {
        "id": "4c31f046b55a7e77",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "ดึงค่าความชื้นจาก Firebase",
        "info": "",
        "x": 698.0000076293945,
        "y": 796.9999694824219,
        "wires": []
    },
    {
        "id": "274dc96832f9f6e9",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "แสดงกราฟความชื้น",
        "info": "",
        "x": 978.0000076293945,
        "y": 756.9999694824219,
        "wires": []
    },
    {
        "id": "e408e5ec02d0540f",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "แสดงกราฟอุณหภูมิ",
        "info": "",
        "x": 968.0000076293945,
        "y": 576.9999694824219,
        "wires": []
    },
    {
        "id": "a2206bbc701cdc28",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "แสดงค่าอุณหภูมิ",
        "info": "",
        "x": 968.0000076293945,
        "y": 656.9999694824219,
        "wires": []
    },
    {
        "id": "a1909f3241b56028",
        "type": "comment",
        "z": "dc457c6f1b613044",
        "name": "แสดงค่าความชื้น",
        "info": "",
        "x": 968.0000076293945,
        "y": 856.9999694824219,
        "wires": []
    },
    {
        "id": "e42253c097a73b8f",
        "type": "ui_text",
        "z": "dc457c6f1b613044",
        "group": "11a858d05f83445d",
        "order": 2,
        "width": 0,
        "height": 0,
        "name": "",
        "label": "Humidity Value",
        "format": "{{msg.payload}} %",
        "layout": "col-center",
        "className": "",
        "x": 968.0000076293945,
        "y": 896.9999694824219,
        "wires": []
    },
    {
        "id": "2c049dcc3a184ca6",
        "type": "ui_text",
        "z": "dc457c6f1b613044",
        "group": "c551eaa1ae24df22",
        "order": 2,
        "width": 0,
        "height": 0,
        "name": "",
        "label": "Temperature Value",
        "format": "{{msg.payload}} C",
        "layout": "col-center",
        "className": "",
        "x": 978.0000076293945,
        "y": 696.9999694824219,
        "wires": []
    },
    {
        "id": "c551eaa1ae24df22",
        "type": "ui_group",
        "name": "Temperature Dashboard",
        "tab": "1cfc07c67ad3c0b9",
        "order": 1,
        "disp": true,
        "width": "6",
        "collapse": false,
        "className": ""
    },
    {
        "id": "11a858d05f83445d",
        "type": "ui_group",
        "name": "Humidity Dashboard",
        "tab": "1cfc07c67ad3c0b9",
        "order": 2,
        "disp": true,
        "width": "6",
        "collapse": false,
        "className": ""
    },
    {
        "id": "acc072ae6bd90de3",
        "type": "database-config",
        "name": "Peerapat Sariman",
        "authType": "email",
        "claims": {},
        "createUser": false,
        "useClaims": false
    },
    {
        "id": "1cfc07c67ad3c0b9",
        "type": "ui_tab",
        "name": "Home",
        "icon": "dashboard",
        "disabled": false,
        "hidden": false
    }
] 

```