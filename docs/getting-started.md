---
title: Getting Started
layout: default
nav_order: 2
---

# Getting Started
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Authentication

Every request to the Asgard API requires a valid **API token**. This token is unique to your account and controls which units and data you can access.

Pass your token as a query parameter on every request:

```
?api_token=YOUR_TOKEN_HERE
```

### Example

```
GET https://integrate.vanguarder.com/chestnut/units?api_token=YOUR_TOKEN_HERE
```

---

## Getting your API token

Your API token is provided by the Vanguarder integration team when your account is set up. If you need a new token or have lost yours, contact [info@vanguarder.com](mailto:info@vanguarder.com).

{: .warning }
Keep your API token secure. Do not expose it in client-side code or public repositories. If you believe your token has been compromised, contact us immediately.

---

## Timestamps

All timestamps in the API are **Unix timestamps** (seconds since 1 January 1970, UTC).

| Field | Format | Example |
|:---|:---|:---|
| `lastContact` | Unix timestamp | `1781597100` |
| `timestamp` | Unix timestamp | `1781597100` |
| `from_ts` / `to_ts` | Unix timestamp | `1781595180` |

To convert a Unix timestamp to a readable date:
- JavaScript: `new Date(1781597100 * 1000).toISOString()`
- Python: `datetime.utcfromtimestamp(1781597100)`
- Online: [https://www.unixtimestamp.com](https://www.unixtimestamp.com)

All times are stored and returned in **UTC**.

---

## Request format

All endpoints use:
- **Method:** `GET`
- **Parameters:** passed as URL query parameters
- **Response:** JSON

---

## Response format

Every response returns a JSON object. Successful responses include a `result` field containing the data. Error responses include a `status` code and `error` message.

### Success
```json
{
  "result": [...],
  "count": 122
}
```

### Error
```json
{
  "result": [],
  "status": "422",
  "error": "unit_id required"
}
```

---

## Polling guidance

The Asgard API is designed to be polled at regular intervals. We recommend:

- **Normal polling interval:** every 60 seconds per unit
- **On reconnection after failure:** always back-fill missed data using the [`/chestnut/positions`](current/chestnut/positions) endpoint before resuming normal polling

### Back-fill pattern

```
1. Store the timestamp of the last successfully received position per unit
2. If a polling cycle fails, do not skip — retry the window
3. On reconnection, call /chestnut/positions with:
     from_ts = last_known_timestamp
     to_ts   = current Unix timestamp
4. Resume normal polling
```

This ensures no data gaps even if the API is temporarily unavailable.
