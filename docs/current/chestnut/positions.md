---
title: GET /chestnut/positions
layout: default
parent: Current API
nav_order: 4
---

# GET /chestnut/positions
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Returns historical GPS positions for a single trailer unit within a specified time window. This endpoint is intended for back-filling data after a polling outage or gap.

{: .new }
This endpoint was added on 30 June 2026. See the [changelog](../../changelog) for details.

---

## Parameters

| Parameter | Type | Required | Default | Description |
|:---|:---|:---|:---|:---|
| `api_token` | string | yes | — | Your API authentication token |
| `unit_id` | integer | yes | — | Unit ID from [`/chestnut/units`](units) |
| `from_ts` | integer | yes | — | Start of time window as Unix timestamp (UTC, inclusive) |
| `to_ts` | integer | yes | — | End of time window as Unix timestamp (UTC, inclusive) |
| `limit` | integer | no | `500` | Maximum number of positions to return per request. Maximum value: `2000` |
| `cursor_id` | integer | no | — | Pagination cursor from a previous response's `next_cursor` field |

---

## Constraints

| Rule | Detail |
|:---|:---|
| Maximum time window | 24 hours (`to_ts - from_ts` must be ≤ 86400 seconds) |
| Maximum `limit` | 2000 |
| Sort order | Ascending by received time |

For windows longer than 24 hours, split into multiple requests.

---

## Example request

```
GET https://integrate.vanguarder.com/chestnut/positions
  ?api_token=YOUR_TOKEN
  &unit_id=5974
  &from_ts=1781595180
  &to_ts=1781597100
```

---

## Paginated request

When the response includes `"has_next": true`, pass the returned `next_cursor` value to retrieve the next page:

```
GET https://integrate.vanguarder.com/chestnut/positions
  ?api_token=YOUR_TOKEN
  &unit_id=5974
  &from_ts=1781595180
  &to_ts=1781597100
  &limit=500
  &cursor_id=84729183
```

---

## Example response

```json
{
  "result": [
    {
      "id": 84729001,
      "timestamp": 1781595183,
      "latitude": 53.48102,
      "longitude": -2.24195,
      "altitude": 44.1,
      "speed": 0,
      "angle": 42,
      "direction": 42,
      "satellites": 14,
      "hdop": 0.8,
      "is_recovery": false
    },
    {
      "id": 84729002,
      "timestamp": 1781595243,
      "latitude": 53.48198,
      "longitude": -2.24261,
      "altitude": 44.5,
      "speed": 5,
      "angle": 38,
      "direction": 38,
      "satellites": 15,
      "hdop": 0.7,
      "is_recovery": false
    }
  ],
  "count": 122,
  "unit_id": 5974,
  "from_ts": 1781595180,
  "to_ts": 1781597100,
  "has_next": false,
  "next_cursor": null
}
```

---

## Response fields

### Top-level

| Field | Type | Description |
|:---|:---|:---|
| `result` | array | Array of position objects, sorted ascending by time |
| `count` | integer | Number of positions in this response |
| `unit_id` | integer | The unit ID queried |
| `from_ts` | integer | Start of the queried window (Unix timestamp, UTC) |
| `to_ts` | integer | End of the queried window (Unix timestamp, UTC) |
| `has_next` | boolean | `true` if more positions exist beyond this page |
| `next_cursor` | integer or null | Pass this as `cursor_id` to retrieve the next page. `null` when `has_next` is `false` |

### Position object

| Field | Type | Description |
|:---|:---|:---|
| `id` | integer | Internal record ID — use it to de-duplicate positions and as the pagination cursor |
| `timestamp` | integer | Unix timestamp (UTC) when the tracker recorded this position (device GPS time) |
| `latitude` | float | Latitude in decimal degrees (WGS84) |
| `longitude` | float | Longitude in decimal degrees (WGS84) |
| `altitude` | float or null | Altitude in metres above sea level |
| `speed` | float or null | Speed in km/h |
| `angle` | float or null | Heading in degrees (0–359, clockwise from north) |
| `direction` | float or null | Same value as `angle`. Kept for backward compatibility |
| `satellites` | integer or null | Number of GPS satellites used for the fix |
| `hdop` | float or null | Horizontal dilution of precision (lower is more accurate) |
| `is_recovery` | boolean | `true` if the position was recovered from the tracker platform after the fact rather than received live |

---

## Pagination

This endpoint uses **keyset pagination**. Results are ordered by `timestamp`, then `id`. Pass the `next_cursor` value from a response as `cursor_id` to continue from the last position returned. Always use the `next_cursor` value exactly as returned — record IDs are not in chronological order, so do not compute a cursor yourself.

```
┌─────────────────────────────────────────────────────────────┐
│  Page 1:  from_ts=T1, to_ts=T2                              │
│           → has_next=true, next_cursor=84729500             │
│                                                             │
│  Page 2:  from_ts=T1, to_ts=T2, cursor_id=84729500         │
│           → has_next=false, next_cursor=null  (done)        │
└─────────────────────────────────────────────────────────────┘
```

Repeat until `has_next` is `false`.

---

## Back-fill pattern

Use this endpoint to recover data after a polling failure:

```python
import requests
import time

BASE_URL = "https://integrate.vanguarder.com"
TOKEN    = "YOUR_TOKEN"
UNIT_ID  = 5974

def backfill(from_ts, to_ts):
    cursor_id = None
    all_positions = []

    while True:
        params = {
            "api_token": TOKEN,
            "unit_id": UNIT_ID,
            "from_ts": from_ts,
            "to_ts": to_ts,
            "limit": 500,
        }
        if cursor_id is not None:
            params["cursor_id"] = cursor_id

        r = requests.get(f"{BASE_URL}/chestnut/positions", params=params)
        data = r.json()

        all_positions.extend(data["result"])

        if not data["has_next"]:
            break

        cursor_id = data["next_cursor"]

    return all_positions
```

---

## Notes

- Only positions with valid GPS coordinates are returned (rows with no GPS fix are excluded)
- `timestamp` is the time the tracker recorded the position (device GPS time), not the time it reached our servers
- Trackers buffer data while they have no mobile signal and send it when the connection returns. Buffered positions can therefore arrive several minutes after their `timestamp`. To catch them, re-query a window that overlaps your previous request (for example the last 30 minutes) and de-duplicate by `id`
- A gap in positions means the tracker did not deliver any GPS fix for that period, for example because of lost mobile or GPS signal. The trailer may still have been moving — compare `speed` either side of the gap
- An invalid or unknown `cursor_id` returns a `422` error with `"error": "Invalid cursor_id"`

---

## Rate limit

60 requests per minute per API token. Exceeding this returns a `429 Too Many Requests` response.
