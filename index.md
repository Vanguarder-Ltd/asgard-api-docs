---
title: Asgard API
layout: home
nav_order: 1
---

# Asgard API Documentation
{: .fs-9 }

The official reference for integrating with the Asgard telematics platform.
{: .fs-6 .fw-300 }

[Get Started](docs/getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View Endpoints](docs/current/overview){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What is Asgard?

Asgard is Vanguarder's telematics platform. It collects real-time GPS, sensor, and diagnostic data from trailer tracking devices and makes it available to integration partners via a secure REST API.

## Available Integrations

| Integration | Base URL | Description |
|:---|:---|:---|
| **Chestnut** | `/chestnut` | Trailer position and status data for telematics platform integrations |
| **Generic** | `/generic` | General-purpose fleet data for third-party integrations |

## Base URL

All API requests go to:

```
https://integrate.vanguarder.com
```

## Need help?

Contact the Vanguarder integration team at [info@vanguarder.com](mailto:info@vanguarder.com)
