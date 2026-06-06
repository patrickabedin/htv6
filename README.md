# htv6 — Hellenic Technologies Marketing Site

Live: **https://ht3-hellenic.vercel.app**
Vercel project: `abedin-1235s-projects / ht3-hellenic`
GitHub: `https://github.com/patrickabedin/htv6`

This repository contains the full source for the Hellenic Technologies marketing site — a content-swapped, JS-stripped static build derived from Linear's design system, rebranded and rebuilt for enterprise digital services.

## Quick Start

```bash
# Deploy to Vercel
vercel deploy --yes --prod --token <VERCEL_TOKEN>
```

No build step. Pure static HTML + CSS. `vercel.json` sets `framework: null` and routes all paths to `index.html`.

## Repository Structure

```
/
├── index.html          # Main homepage (only live page — all others are Linear originals)
├── ht-assets/          # HT brand assets (logo, service images, badges)
├── _next/static/       # Linear's original CSS chunks (unmodified)
├── static/fonts/       # Inter + Berkeley Mono (self-hosted)
├── cdn-cgi/            # Cloudflare image delivery proxy stubs
├── vercel.json         # Vercel config (static, SPA rewrite)
└── docs/               # Developer documentation (this folder)
    ├── design-system.md
    ├── architecture.md
    ├── hero-canvas.md
    └── inner-pages-roadmap.md
```

## Documentation Index

| Doc | What it covers |
|-----|---------------|
| [Design System](docs/design-system.md) | Colors, typography, spacing, components, brand voice |
| [Architecture](docs/architecture.md) | How the static build works, CSS strategy, deployment |
| [Hero Canvas](docs/hero-canvas.md) | Particle network spec, physics, tuning knobs |
| [Inner Pages Roadmap](docs/inner-pages-roadmap.md) | Proposed pages, content structure, reuse patterns |
