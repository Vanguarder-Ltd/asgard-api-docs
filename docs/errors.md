---
title: Error Codes
layout: default
nav_order: 3
---

# Error Codes
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## HTTP Status Codes

| Code | Meaning | Common cause |
|:---|:---|:---|
| `200` | Success | Request completed successfully |
| `403` | Forbidden | Your API token does not have access to the requested unit |
| `404` | Not found | API token not recognised |
| `422` | Unprocessable | Missing or invalid parameter — see `error` field in response |
| `500` | Server error | Internal error — contact [support@vanguarder.com](mailto:support@vanguarder.com) |

---

## 422 — Validation errors

A `422` response always includes an `error` field explaining exactly what is wrong.

| Error message | Cause | Fix |
|:---|:---|:---|
| `api_token required` | No token in request | Add `?api_token=YOUR_TOKEN` |
| `unit_id required` | Missing `unit_id` parameter | Add `&unit_id=5974` |
| `from_ts required` | Missing `from_ts` parameter | Add `&from_ts=1781595180` |
| `to_ts required` | Missing `to_ts` parameter | Add `&to_ts=1781597100` |
| `from_ts must be less than to_ts` | Time window is reversed | Ensure `from_ts` is earlier than `to_ts` |
| `Time window cannot exceed 24 hours` | Window larger than 86400 seconds | Split into multiple requests |
| `unit_id, from_ts, to_ts, limit and cursor_id must be integers` | Non-numeric value passed | Ensure all numeric parameters are integers |
| `Invalid API token` | Token not found or expired | Check token or contact support |

---

## 403 — Access denied

```json
{
  "result": [],
  "status": "403",
  "error": "Access denied to requested unit"
}
```

Your API token is valid but does not have permission to access the requested `unit_id`. Each token is scoped to a specific set of units. Contact [support@vanguarder.com](mailto:support@vanguarder.com) if you believe a unit should be accessible to your account.

---

## 500 — Server error

If you receive a `500` response, please contact [support@vanguarder.com](mailto:support@vanguarder.com) with:
- The full request URL (remove your API token before sending)
- The time of the request (UTC)
- The response body if available
