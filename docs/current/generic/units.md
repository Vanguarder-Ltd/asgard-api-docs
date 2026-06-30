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

| Parameter | Type | Required | Default | Description |
|:---|:---|:---|:---|:---|
| `api_token` | string | yes | — | Your API authentication token |
| `include_location` | boolean | no | `false` | Set to `true` to include a reverse-geocoded address for each unit. Adds latency — omit for high-frequency polling. |

---

## Example request

```
GET https://integrate.vanguarder.com/generic/units?api_token=YOUR_TOKEN
```

With reverse geocoding:

```
GET https://integrate.vanguarder.com/generic/units?api_token=YOUR_TOKEN&include_location=true
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
      "lastContact": 1781597100,
      "odometer": 125430,
      "position": {
        "latitude": 53.48264,
        "longitude": -2.24382,
        "altitude": 45.2,
        "angle": 217,
        "speed": 0,
        "ignition": false
      },
      "Events": "No Issues",
      "location": null
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
| `lastContact` | integer or null | Unix timestamp (UTC) of last data received |
| `odometer` | float or null | Odometer reading at time of last contact |
| `position` | object or null | Last known position (see below) |
| `Events` | string | `"No Issues"` or `"Issues"` — indicates active DTC or overweight events |
| `location` | string or null | Reverse-geocoded address — only populated when `include_location=true` |

### `position` object

| Field | Type | Description |
|:---|:---|:---|
| `latitude` | float | Latitude in decimal degrees (WGS84) |
| `longitude` | float | Longitude in decimal degrees (WGS84) |
| `altitude` | float | Altitude in metres above sea level |
| `angle` | integer | Heading in degrees (0–359, clockwise from north) |
| `speed` | integer | Speed in km/h |
| `ignition` | boolean | Whether ignition was active |
