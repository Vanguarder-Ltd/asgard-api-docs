---
title: Changelog
layout: default
nav_order: 4
---

# Changelog

All notable changes to the Asgard API are documented here.

---

## 5 October 2026

### Added
- **`GET /chestnut/positions`** — Each position now includes `angle` (heading, as documented) and `altitude`. `satellites`, `hdop` and `is_recovery` are now documented.
- **`GET /generic/units`, `GET /generic/unit`** — The `position` object now includes `angle`.

### Fixed
- **`GET /chestnut/positions`** — Pagination could skip or repeat positions when buffered or recovered data was stored out of order. The cursor now follows the result order (`timestamp`, then `id`). The `cursor_id` / `next_cursor` parameters are unchanged.
- **Documentation** — `timestamp` is the device GPS time, not the server receive time. Removed references to an `ignition` field, which is not provided by the API. Added guidance on re-querying an overlapping window to catch late-arriving buffered positions.

### Unchanged
- `direction` is still returned alongside `angle` for backward compatibility.

---

## 30 June 2026

### Added
- **`GET /chestnut/positions`** — New endpoint returning historical GPS positions for a unit within a specified time window. Supports keyset pagination via `cursor_id`. Maximum window of 24 hours per request. This endpoint enables integration partners to back-fill missed data after a polling outage.

---

## Upcoming

### Planned
- **API v1** — Versioned endpoints under `/v1/` for long-term stability guarantees. Existing endpoints will remain fully supported. Announcement will be made in this changelog before any changes take effect.

---

{: .note }
Breaking changes will always be announced in this changelog with a minimum 60-day notice period before taking effect. Existing endpoints will never be removed without prior notice.
