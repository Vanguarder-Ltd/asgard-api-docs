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
        "direction": 217,
        "speed": 0,
        "satellites": 14,
        "hdop": 0.8
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

The last position reported by the tracker. Fields depend on the tracker model; the common ones are:

| Field | Type | Description |
|:---|:---|:---|
| `latitude` | float | Latitude in decimal degrees (WGS84) |
| `longitude` | float | Longitude in decimal degrees (WGS84) |
| `altitude` | float | Altitude in metres above sea level |
| `angle` | float | Heading in degrees (0–359, clockwise from north) |
| `direction` | float | Same value as `angle`. Kept for backward compatibility |
| `speed` | float | Speed in km/h |
| `satellites` | integer | Number of GPS satellites used for the fix |
| `hdop` | float | Horizontal dilution of precision (lower is more accurate) |

{: .note }
`lastContact` only advances when the tracker delivers new data. If a tracker loses mobile signal, `lastContact` and `position` stay at the last received values until it reconnects. Use [`/chestnut/positions`](../chestnut/positions) to back-fill the full track afterwards.

---

## Rate limit

60 requests per minute per API token. Exceeding this returns a `429 Too Many Requests` response.
