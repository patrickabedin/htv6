# HTv6 — Hellenic Technologies Demo Site

Live demo: https://ht3-hellenic.vercel.app

This is a demo site built on Linear.app's design system with Hellenic Technologies content.

## Structure
- `index.html` — patched static HTML (Linear layout + HT content)
- `_next/static/` — CSS and JS chunks from Linear's Next.js build
- `ht-assets/` — HT logo, service images, partner badges
- `cdn-cgi/imagedelivery/local/` — locally cached Cloudflare images
- `static/fonts/` — Inter Variable, Berkeley Mono fonts
- `vercel.json` — Vercel static deployment config

## Deploy
```bash
vercel deploy --prod
```

## Backup / Revert
Full nginx backup at `/var/www/ht3-backup-20260606-085307` on staging droplet (209.38.99.80).
