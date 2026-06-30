---
title: GET /generic/units
layout: default
parent: Current API
nav_order: 5
---

# GET /generic/units
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Returns all units accessible to your API token, including last known position and sensor data for each.

---

## Parameters

| Parameter | Type | Required | Description |
|:---|:---|:---|:---|
| `api_token` | string | yes | Your API authentication token |

---

## Example request

```
GET https://integratevanguarder.com/generic/units?api_token=YOUR_TOKEN
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
| `id` | integer | Unique unit ID in Asgard |
| `name` | string | Trailer identifier (e.g. fleet number) |
| `ident` | string | Device IMEI number |
| `plate` | string | Vehicle registration plate — may be empty |
| `latitude` | float | Last known latitude in decimal degrees (WGS84) |
| `longitude` | float | Last known longitude in decimal degrees (WGS84) |
| `altitude` | float | Altitude in metres above sea level |
| `angle` | integer | Heading in degrees (0–359, clockwise from north) |
| `speed` | integer | Speed in km/h at time of last contact |
| `ignition` | boolean | Whether ignition was active at last contact |
| `lastContact` | integer | Unix timestamp (UTC) of last data received |
