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

| Parameter | Type | Required | Default | Description |
|:---|:---|:---|:---|:---|
| `api_token` | string | yes | — | Your API authentication token |
| `name` | string | yes | — | Trailer name or partial name (case-insensitive). Must match exactly one unit. |
| `include_location` | boolean | no | `false` | Set to `true` to include a reverse-geocoded address. Adds latency — omit for high-frequency polling. |

---

## Example request

```
GET https://integrate.vanguarder.com/generic/unit?api_token=YOUR_TOKEN&name=44391-18
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
      "Events": "No Issues",
      "location": null,
      "position": {
        "latitude": 53.48264,
        "longitude": -2.24382,
        "altitude": 45.2,
        "angle": 217,
        "speed": 0,
        "ignition": false
      }
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
| `lastContact` | integer or null | Unix timestamp (UTC) of last data received |
| `odometer` | float or null | Odometer reading at time of last contact |
| `Events` | string | `"No Issues"` or `"Issues"` — indicates active DTC or overweight events |
| `location` | string or null | Reverse-geocoded address — only populated when `include_location=true` |
| `position` | object or null | Last known position (see below) |

### `position` object

| Field | Type | Description |
|:---|:---|:---|
| `latitude` | float | Latitude in decimal degrees (WGS84) |
| `longitude` | float | Longitude in decimal degrees (WGS84) |
| `altitude` | float | Altitude in metres above sea level |
| `angle` | integer | Heading in degrees (0–359, clockwise from north) |
| `speed` | integer | Speed in km/h |
| `ignition` | boolean | Whether ignition was active |

---

## Notes

- The `name` parameter performs a partial, case-insensitive match
- If the search matches more than one unit, a `422` error is returned — use a more specific name
