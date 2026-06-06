# Architecture — htv6

## Overview

htv6 is a **pure static HTML site** — no build pipeline, no framework, no Node runtime. It is a content-replaced, JS-stripped fork of Linear's public marketing site, rebuilt as a branded Hellenic Technologies property.

---

## Why Static (No Framework)

Linear's site is a Next.js SSR app. When we replaced text content, the React hydration checksum no longer matched the server-rendered HTML, causing a blank page crash (React error #306).

**Resolution:** Strip all Next.js JS bundles. Keep only the CSS. Serve as pure static HTML.

This means:
- No client-side routing
- No React, no JS framework
- Linear's CSS classes are preserved exactly (they're in the unmodified CSS chunks)
- Custom behaviour is injected as self-contained `<script>` blocks inside the HTML

---

## File Structure

```
/
├── index.html              ← The homepage — all edits go here
├── vercel.json             ← Deployment config
├── .vercel/                ← Vercel project binding
├── .gitignore
│
├── ht-assets/              ← HT brand assets
│   ├── ht-logo.svg         ← HT wordmark (white)
│   ├── falcon.webp         ← Web Development service image
│   ├── lynx.webp           ← Website Support service image
│   ├── mammoth.webp        ← Performance Marketing service image
│   ├── samurai.webp        ← SEO service image
│   ├── knight.webp         ← Social Media service image
│   ├── apache.webp         ← Creative Design service image
│   ├── ht-hero-bg.jpg      ← Hero background fallback
│   └── badge-*.svg         ← Partner/trust badges
│
├── _next/
│   └── static/
│       ├── css/            ← Linear's CSS chunks (18 files, ~1.4MB total)
│       └── media/          ← Audio asset (pulse.mp3)
│
├── static/
│   └── fonts/              ← Inter + Berkeley Mono (self-hosted)
│
└── cdn-cgi/
    └── imagedelivery/      ← Cloudflare image proxy stubs
```

---

## CSS Strategy

Linear's CSS is used **unmodified**. All HT customisations are injected via a `<style>` block near `</head>`:

```html
<style>
  /* Force Next.js lazy-load overrides */
  img[data-nimg] { opacity: 1 !important; }
  img[loading="lazy"] { opacity: 1 !important; }

  /* Animation overrides — strip JS-dependent entrance states */
  [data-loaded="false"] { opacity: 1 !important; filter: none !important; }

  /* HT nav styles */
  .ht-nav-a { ... }
  .ht-drop  { ... }
  /* etc */
</style>
```

**Do not edit the CSS chunks in `_next/static/css/`** — they are minified and difficult to read. Override everything in the injected style block.

---

## Key HTML Patches Applied to index.html

| Patch | Location | Technique |
|-------|----------|-----------|
| Strip Next.js JS | `<head>` | Removed all `<script>` tags pointing to `/_next/static/chunks/` |
| Force image visibility | `<head>` | CSS override on `img[data-nimg]`, `[data-loaded]` |
| HT logo | `<header>` | Replaced Linear SVG with `<img src="/ht-assets/ht-logo.svg">` |
| HT nav + burger | `<header>` | Full header replaced with custom HTML block |
| Hero H1 text | Hero section | Text node replacements in animated `<span>` elements |
| Hero spacer gap | Above H1 | `--height:200px → 60px` on first Spacer div |
| Hero visual | `.Frame_wrapper___hKDg` | Replaced with `<canvas id="ht-particles">` block |
| Carousel pills | `.Carousel_inner__L4Uvf` | Replaced 3 Linear pills with 10 HT service pills |
| Feature sections | Various | Text replacements, stat numbers, feature names |
| Testimonials | Testimonial section | Full copy replacement (names, roles, quotes) |
| Footer CTA | Pre-footer section | Headline + subtext replaced |
| JS injection | Before `</body>` | Particle canvas script, nav dropdown JS, burger JS |

---

## Deployment

### Vercel

```bash
cd /path/to/htv6
vercel deploy --yes --prod --token <TOKEN>
```

`vercel.json`:
```json
{
  "framework": null,
  "outputDirectory": ".",
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

Project: `abedin-1235s-projects/ht3-hellenic`  
Live URL: `https://ht3-hellenic.vercel.app`

### GitHub

Repo: `https://github.com/patrickabedin/htv6` (public)  
Push method: GitHub Contents API (base64 PUT) — `git push` hangs in this sandbox.

```bash
# Push a single file via API
FILE_CONTENT=$(base64 -w0 path/to/file)
SHA=$(curl -s -H "Authorization: token $GH_TOKEN" \
  "https://api.github.com/repos/patrickabedin/htv6/contents/path/to/file" \
  | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))")

curl -s -X PUT \
  -H "Authorization: token $GH_TOKEN" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/patrickabedin/htv6/contents/path/to/file" \
  -d "{\"message\":\"commit message\",\"content\":\"$FILE_CONTENT\",\"sha\":\"$SHA\"}"
```

---

## Making Changes

### Edit Flow

1. Work on `/tmp/ht3-vN.html` (increment version number)
2. Run Python patch scripts to apply changes
3. Copy to `/tmp/ht3-vercel/index.html`
4. `vercel deploy --yes --prod` to test live
5. Copy to `/tmp/htv6/index.html`
6. Push to GitHub via Contents API

### Common Gotchas

- **Never edit `_next/static/css/`** — override via injected `<style>` block
- **Image paths must be root-relative** — `/ht-assets/logo.svg` not `./ht-assets/`
- **CDN images** — Linear section images that aren't replaced still point to `linear.app/cdn-cgi/imagedelivery/` live URLs. If those break, replacements are needed.
- **`data-loaded` attribute** — Linear's JS sets this to `"true"` after load. Since we stripped JS, force it in CSS or HTML: `data-loaded="true"`
- **Canvas resize** — mobile Chrome fires `window.resize` on address-bar show/hide. The canvas resize handler debounces 150ms and skips if only the height changed (width delta < 10px)

---

## Local Preview

No build needed. Open directly in browser or serve with any static server:

```bash
cd /tmp/ht3-vercel
python3 -m http.server 8080
# Open http://localhost:8080
```
