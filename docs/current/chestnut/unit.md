---
title: GET /chestnut/unit
layout: default
parent: Current API
nav_order: 3
---

# GET /chestnut/unit
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Returns the last known position and status for a single trailer unit.

---

## Parameters

| Parameter | Type | Required | Description |
|:---|:---|:---|:---|
| `api_token` | string | yes | Your API authentication token |
| `name` | string | yes | Trailer name or partial name (case-insensitive). Must match exactly one unit. |

---

## Example request

```
GET https://integrate.vanguarder.com/chestnut/unit?api_token=YOUR_TOKEN&name=44391-18
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
      "location": "Trafford Park, Manchester, UK"
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
| `lastContact` | integer | Unix timestamp (UTC) of last data received |
| `odometer` | float or null | Odometer reading at time of last contact |
| `Events` | string | `"No Issues"` or `"Issues"` — indicates active DTC or overweight events |
| `location` | string or null | Reverse-geocoded address of last known position |

---

## Notes

- The `name` parameter performs a partial, case-insensitive match
- If the search matches more than one unit, a `422` error is returned — use a more specific name
- `location` is always included as a human-readable address string when a valid position is available

## Notes

- This endpoint returns only the **most recent** record for the unit
- For historical positions within a time window, use [`/chestnut/positions`](positions)
- If a unit has not transmitted recently, `lastContact` may be several hours old
