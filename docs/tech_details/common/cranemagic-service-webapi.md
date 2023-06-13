# WebAPI

## Realtime

### `/realtime/generateCraneTaskHanding/DP-Kunshan`

Payload

```json
{
  "crane": {
    "id": "85",
    "posX": "002874",
    "posY": "005243",
    "posZ": "000394",
    "lCarSpeed": "06835",
    "sCarSpeed": "00425",
    "hookSpeed": "00092",
    "hookWeight": "00500",
    "taskId": "20230602111714",
    "workStatus": 0,
    "faultCode": "E3",
    "faultDescription": "",
    "eventDate": "2023-06-12 13:56:25",
    "info": {
      "id": "85",
      "type": "CRANE",
      "model": {
        "id": "DP-CRANE-Kunshan",
        "name": "东普昆山起重机型号",
        "properties": {
          "lCar_width": 8000,
          "sCar_width": 4000,
          "lCar_length": 22000,
          "sCar_length": 5400,
          "hook_maxSpeed": 1500,
          "lCar_maxSpeed": 1500,
          "sCar_maxSpeed": 1500
        }
      },
      "usage": {
        "status": "INUSE",
        "purchaseTime": "2018-11-21 00:00:00",
        "startTime": null,
        "lastModifiedTime": null,
        "expiredTime": null
      },
      "comment": "",
      "maxHeight": 3000,
      "warehouse": { "id": "DP-Kunshan" }
    }
  },
  "sourceArea": [
    "IN",
    {
      "id": "In",
      "type": "IN",
      "name": "对中平台",
      "size": { "length": 10000, "width": 2500, "height": 880 },
      "position": { "xAxis": 6549, "yAxis": 19516, "zAxis": 0, "degree": 0 },
      "centerPosition": { "xAxis": 6549, "yAxis": 19516, "zAxis": 0 },
      "comment": "",
      "warehouse": { "id": "DP-Kunshan" },
      "material_count": 2
    }
  ],
  "targetArea": [
    "STORAGE",
    {
      "id": "KW01",
      "type": "STORAGE",
      "name": "KW01",
      "size": { "length": 9000, "width": 2200, "height": 1550 },
      "position": { "xAxis": 1600, "yAxis": 15580, "zAxis": 0, "degree": 0 },
      "centerPosition": { "xAxis": 1600, "yAxis": 15580, "zAxis": 0 },
      "comment": "kkkkkk0001",
      "warehouse": { "id": "DP-Kunshan" },
      "material_count": 1
    }
  ],
  "materials": [
    {
      "id": "AT-989797",
      "model": {
        "id": "kkkkkk0001",
        "name": "钢板6/Q235B",
        "raws": "钢板6",
        "thickness": "Q235B",
        "size": { "length": 6000, "width": 1720, "height": 6 },
        "delta": { "xAxis": 1800, "yAxis": 0, "zAxis": 0 }
      },
      "area": { "id": "In", "type": "IN", "name": "对中平台", "comment": "" },
      "areaSeq": 0,
      "createTime": "2023-06-09 19:52:38",
      "removeTime": null,
      "info": "AT-989797,6000,1720,6,0"
    },
    {
      "id": "AT-989796",
      "model": {
        "id": "kkkkkk0001",
        "name": "钢板6/Q235B",
        "raws": "钢板6",
        "thickness": "Q235B",
        "size": { "length": 6000, "width": 1720, "height": 6 },
        "delta": { "xAxis": 1800, "yAxis": 0, "zAxis": 0 }
      },
      "area": { "id": "In", "type": "IN", "name": "对中平台", "comment": "" },
      "areaSeq": 1,
      "createTime": "2023-06-09 19:52:38",
      "removeTime": null,
      "info": "AT-989796,6000,1720,6,0"
    }
  ]
}
```
