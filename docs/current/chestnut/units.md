---
title: GET /chestnut/units
layout: default
parent: Current API
nav_order: 2
---

# GET /chestnut/units
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Returns a list of all trailer units accessible to your API token.

---

## Parameters

| Parameter | Type | Required | Description |
|:---|:---|:---|:---|
| `api_token` | string | yes | Your API authentication token |

---

## Example request

```
GET https://integratevanguarder.com/chestnut/units?api_token=YOUR_TOKEN
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
      "plate": ""
    },
    {
      "id": 5975,
      "name": "44392-18",
      "ident": "866233059354531",
      "plate": ""
    }
  ]
}
```

---

## Response fields

| Field | Type | Description |
|:---|:---|:---|
| `id` | integer | Unique unit ID in Asgard — use this as `unit_id` in other endpoints |
| `name` | string | Trailer identifier (e.g. fleet number) |
| `ident` | string | Device IMEI number |
| `plate` | string | Vehicle registration plate — may be empty |

---

## Notes

- Only units assigned to your API token are returned
- Use the `id` field as `unit_id` when calling [`/chestnut/unit`](unit) or [`/chestnut/positions`](positions)
