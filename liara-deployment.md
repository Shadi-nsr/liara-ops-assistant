# Liara deployment

- App: `liara-ops-assistant`
- Public URL: https://liara-ops-assistant.liara.run
- Platform: Docker
- Port: `8080`
- Plan: زمین / 0.5 GB RAM / 0.5 Core / 5 GB Disk
- Build: موفق
- Health: `GET /api/health` returned `200`
- The product remains offline-safe: actions are `dry-run` and no real Liara mutation is performed by the app.
