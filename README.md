# Asgard API Documentation

Official API reference for Vanguarder integration partners.

**Live site:** https://vanguarder-ltd.github.io/asgard-api-docs

## What's documented

- `GET /chestnut/units` — list all units
- `GET /chestnut/unit` — single unit status
- `GET /chestnut/positions` — historical GPS positions with pagination
- `GET /generic/units` — units with position and event data
- `GET /generic/unit` — single unit with position and event data

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the branch strategy and commit conventions.

**Branch model:**
- `main` — production, auto-deploys to GitHub Pages on every push
- `develop` — staging, merge feature branches here first
- `feat/*` / `fix/*` / `docs/*` — working branches, squash-merge into develop
