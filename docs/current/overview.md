---
title: Overview
layout: default
parent: Current API
nav_order: 1
---

# Current API — Overview

The current Asgard API provides two integration flavours depending on your use case.

| Integration | Base path | Best for |
|:---|:---|:---|
| **Chestnut** | `/chestnut` | Trailer positions, status, and historical back-fill for telematics integrations |
| **Generic** | `/generic` | General fleet integrations needing unit list and last known position |

---

## Chestnut endpoints

| Method | Endpoint | Description |
|:---|:---|:---|
| `GET` | [`/chestnut/units`](chestnut/units) | List all units accessible to your account |
| `GET` | [`/chestnut/unit`](chestnut/unit) | Get last known status for a single unit |
| `GET` | [`/chestnut/positions`](chestnut/positions) | Get historical positions for a unit within a time window |

## Generic endpoints

| Method | Endpoint | Description |
|:---|:---|:---|
| `GET` | [`/generic/units`](generic/units) | List all units with last position and sensor data |
| `GET` | [`/generic/unit`](generic/unit) | Get last known status for a single unit |
