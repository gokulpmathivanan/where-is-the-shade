# Where is the shade?

Tells you which side of the bus or train to sit on to stay out of the sun.
Installable PWA. No API keys anywhere.

- Transit routing: [Transitous](https://transitous.org) (community GTFS, worldwide)
- Cloud forecast: [Open-Meteo](https://open-meteo.com)
- Geocoding: OpenStreetMap Nominatim; road fallback: OSRM
- Sun position: built-in solar calculation (validated against suncalc, <1°)

## Layout

- `app/` — the entire application (static files, no build step)
- `shade.conf` — nginx config (manifest MIME type, no-cache for updates)
- `docker-compose.yml` — nginx:alpine on host port 8601

## Deploy

```bash
docker compose up -d
curl -I http://localhost:8601        # expect 200 OK
```

Then route a hostname to port 8601 (e.g. a cloudflared ingress rule) and
open it on your phone — Add to Home Screen installs it.

## Update loop

Edit `app/index.html`, commit, then on the server:

```bash
git pull
```

No rebuild, no restart — nginx serves the mounted files directly, and the
service worker is network-first, so phones pick up changes on next open.
