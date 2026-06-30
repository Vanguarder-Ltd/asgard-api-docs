---
title: GET /generic/unit
layout: default
parent: Current API
nav_order: 6
---

# GET /generic/unit
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Returns the last known position and status for a single unit.

---

## Parameters

| Parameter | Type | Required | Description |
|:---|:---|:---|:---|
| `api_token` | string | yes | Your API authentication token |
| `unit_id` | integer | yes | Unit ID from [`/generic/units`](units) |

---

## Example request

```
GET https://integratevanguarder.com/generic/unit?api_token=YOUR_TOKEN&unit_id=5974
```

---

## Example response

```json
{
  "result": [
    {
      "id": 5974,
      "name": "44391-18",
      "ident": "866233059354530",
      "plate": "",
      "latitude": 53.48264,
      "longitude": -2.24382,
      "altitude": 45.2,
      "angle": 217,
      "speed": 0,
      "ignition": false,
      "lastContact": 1781597100
    }
  ]
}
```

---

## Response fields

| Field | Type | Description |
|:---|:---|:---|
| `id` | integer | Unit ID |
| `name` | string | Trailer identifier |
| `ident` | string | Device IMEI |
| `plate` | string | Registration plate — may be empty |
| `latitude` | float | Last known latitude in decimal degrees (WGS84) |
| `longitude` | float | Last known longitude in decimal degrees (WGS84) |
| `altitude` | float | Altitude in metres above sea level |
| `angle` | integer | Heading in degrees (0–359, clockwise from north) |
| `speed` | integer | Speed in km/h at time of last contact |
| `ignition` | boolean | Whether ignition was active at last contact |
| `lastContact` | integer | Unix timestamp (UTC) of last data received |
