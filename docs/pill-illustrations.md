# Carousel Pill SVG Illustrations

How to create or update the 10 isometric SVG illustrations used in the service carousel pills.

## The Spec

Each pill uses an **inline SVG** (not an image file) injected directly into the `Benefits_carouselIllustration__674X3` div.

```
width="265" height="262" viewBox="0 0 265 262" fill="none"
```

Transparent background — no rect fill on the root SVG.

## Color Palette (exact, no substitutions)

| Color | Hex | Usage |
|-------|-----|-------|
| Dark fill | `#08090A` | All shape faces / fills |
| Grid lines | `#3E3E44` | Grid lines, dashed connectors, structural lines |
| Top highlight | `#D0D6E0` | Top face edges only — used sparingly (5–8x per SVG) |

> **Note:** SVG 3 in Linear's original also uses `#62666D` face strokes and `#2E2E32` detail lines — slightly lighter variants. Either palette works; `#3E3E44` is the primary.

## Structure Rules

1. **Full bleed** — content must span nearly the full canvas. X range: ~15–250, Y range: ~40–245. If content is clustered in the center it will render at half visual size.
2. **stroke-width="0.5"** on everything — no exceptions.
3. **`filter="brightness(1)"`** on shape groups — wrap each isometric slab/layer in `<g stroke-width="0.5" filter="brightness(1)">`.
4. **40+ elements** — each SVG should be geometrically rich, not minimal.
5. **No text, no gradients, no photos, no color fills** except `#08090A`.

## Shape Grammar

Each illustration is built from these isometric primitives:

```svg
<!-- Isometric slab (the core building block) -->
<g stroke-width="0.5" filter="brightness(1)">
  <path fill="#08090A" stroke="#3E3E44" d="M... (diamond/hex/rect face)"/>
  <path stroke="#3E3E44" d="M... (vertical edge)"/>
  <path stroke="#D0D6E0" d="M... (top face highlight — top edge only)"/>
</g>

<!-- Grid floor lines -->
<g stroke="#3E3E44" stroke-width="0.5" stroke-linecap="round">
  <path d="m15 200 117.5 47 117.5-47"/>  <!-- isometric row -->
  ...
</g>

<!-- Dashed connectors between layers -->
<line stroke="#3E3E44" stroke-width="0.5" stroke-dasharray="3 3" .../>
```

## The 10 Slugs and Metaphors

| Slug | Service | Metaphor |
|------|---------|----------|
| `ai` | AI | Neural network — nodes + radial connections + central hub |
| `bi` | Business Intelligence | Bar chart — isometric 3D columns ascending L→R |
| `web-dev` | Web Development | Stacked panels — layered isometric rectangles (UI layers) |
| `web-support` | Website Support | Hex shield — concentric hexagonal prisms |
| `perf-marketing` | Performance Marketing | Rising curve — grid floor + ascending line plot with nodes |
| `web-analytics` | Web Analytics | Funnel — stacked rings decreasing bottom→top |
| `compliance` | Digital Compliance | Document stack — 3 offset isometric rectangle slabs |
| `social` | Social Media Management | Social graph — radial spokes to 8 outer nodes |
| `seo` | SEO | Staircase podium — 3-step isometric staircase L→R |
| `creative` | Creative Design | Design canvas — grid surface + vector path with anchors |

## How to Inject Into HTML

The SVG goes directly inside the illustration div — no `<img>` tags:

```python
# Python injection script pattern
svg_content = open('pill-ai.svg').read().strip()
new_div = f'<div class="Benefits_carouselIllustration__674X3">{svg_content}</div>'

# Find the existing illustration div for a given service using its h3 heading
# marker = f'<h3 class="sc-d5151d0-0 kEiGEn">{service_name}</h3>'
# Walk backward to the Benefits_carouselIllustration__674X3 div before it
# Replace the whole div and its contents with new_div
```

See `docs/architecture.md` for the full `replace_illustration()` function pattern.

## What NOT to Do

- ❌ Do not use `image_generate` tool — routes to wrong model (Gemini Flash), produces colorful photos
- ❌ Do not use `<img src="...">` — images will be colorful/cropped/wrong style
- ❌ Do not center content in a small region — must be full bleed or it looks half-size
- ❌ Do not add `min-height` or `padding` inline styles to the illustration div — let Linear's CSS handle layout
- ❌ Do not use `#3E3E44` as the only stroke — pair it with `#D0D6E0` highlights or shapes disappear against dark bg

## Deployment

```bash
# After updating index.html with new SVGs:
cd /tmp/ht3-vercel
cp /tmp/ht3-v{N}.html index.html
vercel deploy --yes --prod --token <token>

# Push to GitHub:
cd /tmp/htv6
git add index.html
git commit -m "vN: updated pill illustrations"
git push https://<token>@github.com/patrickabedin/htv6.git master
```
