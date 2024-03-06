```ruby

[
    {
        "id": "dfc62b58feb885f4",
        "type": "tab",
        "label": "บท7",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "bfe6de8f2ab2bd5d",
        "type": "firebase-in",
        "z": "dfc62b58feb885f4",
        "name": "",
        "constraint": {},
        "database": "ca5028033c5a512e",
        "listenerType": "value",
        "outputType": "auto",
        "path": "/GPS",
        "useConstraint": false,
        "x": 550,
        "y": 400,
        "wires": [
            [
                "10d051dfd8616749",
                "d487de2fd381eb42"
            ]
        ]
    },
    {
        "id": "d487de2fd381eb42",
        "type": "worldmap",
        "z": "dfc62b58feb885f4",
        "name": "",
        "lat": "",
        "lon": "",
        "zoom": "",
        "layer": "OSMG",
        "cluster": "",
        "maxage": "",
        "usermenu": "show",
        "layers": "show",
        "panit": "true",
        "panlock": "false",
        "zoomlock": "false",
        "hiderightclick": "false",
        "coords": "none",
        "showgrid": "false",
        "showruler": "false",
        "allowFileDrop": "false",
        "path": "/worldmap",
        "overlist": "DR,CO,RA,DN,HM",
        "maplist": "OSMG,OSMC,EsriC,EsriS,EsriT,EsriDG,UKOS",
        "mapname": "",
        "mapurl": "",
        "mapopt": "",
        "mapwms": false,
        "x": 900,
        "y": 420,
        "wires": []
    },
    {
        "id": "10d051dfd8616749",
        "type": "debug",
        "z": "dfc62b58feb885f4",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 880,
        "y": 580,
        "wires": []
    },
    {
        "id": "ca5028033c5a512e",
        "type": "database-config",
        "name": "My Database",
        "authType": "email",
        "claims": {},
        "createUser": false,
        "useClaims": false
    }
]

```