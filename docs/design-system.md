# Design System — Hellenic Technologies

This document defines the complete visual language for the HT marketing site. All inner pages must follow these specs to maintain consistency with the homepage.

---

## Brand Identity

**Name:** Hellenic Technologies  
**Tagline:** Enterprise digital services, engineered for growth  
**Logo:** `/ht-assets/ht-logo.svg` — display at `height: 20px`, `width: auto`  
**Logo link:** `https://hellenictechnologies.com`

---

## Color Palette

### Core Backgrounds

| Token | Value | Usage |
|-------|-------|-------|
| `bg-base` | `#07070d` | Canvas/hero backgrounds |
| `bg-surface` | `#0a0a10` | Page background |
| `bg-card` | `#0f0f18` | Card / section backgrounds |
| `bg-elevated` | `#16181f` | Dropdown, modal, elevated surfaces |
| `border-subtle` | `rgba(255,255,255,0.06)` | Default card/section borders |
| `border-mid` | `rgba(255,255,255,0.10)` | Stronger dividers |

### Text

| Token | Value | Usage |
|-------|-------|-------|
| `text-primary` | `rgba(255,255,255,0.92)` | Headlines, primary content |
| `text-secondary` | `rgba(255,255,255,0.55)` | Body copy, descriptions |
| `text-muted` | `rgba(255,255,255,0.35)` | Labels, metadata, captions |
| `text-nav` | `rgba(255,255,255,0.50)` | Navigation links (resting) |
| `text-nav-hover` | `rgba(255,255,255,0.85)` | Navigation links (hover) |

### Accent — Indigo Family (primary brand accent)

| Token | Value | Usage |
|-------|-------|-------|
| `accent-base` | `#6366f1` | Primary CTA, active states |
| `accent-bright` | `rgba(165,180,252,1)` | Particle canvas accent nodes, highlights |
| `accent-dim` | `rgba(148,163,184,1)` | Particle canvas base nodes, secondary accents |
| `accent-glow` | `rgba(94,106,210,0.08)` | Aurora/radial glow backgrounds |

### Status / Functional

| Token | Value | Usage |
|-------|-------|-------|
| `green` | `#4ade80` | Success, uptime indicators |
| `amber` | `#fbbf24` | Warnings, upgrade banners |
| `red` | `#f87171` | Errors, alerts |

---

## Typography

The site inherits Linear's type stack. Do not introduce new fonts.

### Font Families

```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
font-family: 'Berkeley Mono', 'Fira Code', monospace; /* code blocks only */
```

Both fonts are self-hosted under `/static/fonts/`.

### Type Scale

| Role | Size | Weight | Line Height | Letter Spacing |
|------|------|--------|-------------|----------------|
| Hero H1 | 56–64px | 600 | 1.08 | -0.03em |
| Section H2 | 36–42px | 600 | 1.1 | -0.02em |
| Card H3 | 22–26px | 600 | 1.2 | -0.02em |
| Body large | 18px | 400 | 1.65 | -0.01em |
| Body default | 15–16px | 400 | 1.6 | -0.01em |
| Caption / label | 12–13px | 400–500 | 1.4 | 0 |
| Nav links | 13px | 400 | — | -0.01em |
| Canvas labels | 10–11px | 400–500 | — | 0 |

### Copy Rules

- **No hyphens** in service or product names
- **No exclamation marks** anywhere
- **No em dashes** — use commas or restructure
- **Enterprise verbs:** deploy, engineer, optimise, accelerate, orchestrate, integrate, automate
- **Tone:** confident, direct, zero fluff — like a premium agency, not a startup
- Sentences should be declarative, not questions or calls to action with energy

---

## Spacing System

Linear's layout uses a base-8 spacing scale. Stick to multiples of 8.

| Step | Value | Usage |
|------|-------|-------|
| xs | 4px | Internal padding, icon gaps |
| sm | 8px | Component gaps |
| md | 16px | Section inner padding |
| lg | 24px | Card padding, section gaps |
| xl | 40px | Section vertical spacing |
| 2xl | 64px | Major section breaks |
| 3xl | 96–120px | Hero / full-bleed section padding |

Max content width: `1200px`, centered, `padding: 0 24px`.

---

## Components

### Navigation Header

```
position: fixed; top: 0; left: 0; right: 0; z-index: 1000
background: rgba(10,10,15,0.92); backdrop-filter: blur(20px)
border-bottom: 1px solid rgba(255,255,255,0.07)
height: 60px
```

- Logo left-aligned at `height: 20px`
- Nav links centered: `13px`, weight 400, `rgba(255,255,255,0.5)` resting → `0.85` hover
- Dropdowns: `background: #16181f`, `border-radius: 10px`, `border: 1px solid rgba(255,255,255,0.1)`
- Burger menu: `display: none` on desktop (`>768px`), `display: flex` on mobile
- Burger animates to X on open (bars transform + middle fades)
- Mobile drawer: full-screen, `background: rgba(8,8,14,0.98)`, `backdrop-filter: blur(20px)`

### Cards

```css
background: #0f0f18;
border: 1px solid rgba(255,255,255,0.06);
border-radius: 12px;
padding: 24px;
```

Hover state: `border-color: rgba(255,255,255,0.12)`, `background: #131320`

### Carousel / Pill Cards

Linear's `.Benefits_carouselCard__Fjt3f` — preserve the class. Each pill has:
- Illustration area (top): `height ~130px`, dark bg, centred SVG or canvas visual
- Label: `13–14px`, medium weight
- Description: `13px`, muted

### Buttons

**Primary:**
```css
background: rgba(255,255,255,0.08);
border: 1px solid rgba(255,255,255,0.1);
color: rgba(255,255,255,0.85);
border-radius: 8px;
padding: 8px 18px;
font-size: 13px;
font-weight: 500;
```

**Ghost nav button** (same as above but `background: none`, appears on hover only)

**CTA (high-emphasis):**
```css
background: #6366f1;
color: #fff;
border-radius: 8px;
padding: 12px 24px;
font-size: 15px;
font-weight: 500;
```

### Section Dividers

```css
border-top: 1px solid rgba(255,255,255,0.06);
```

### Glow / Aurora Backgrounds

For section backgrounds with a subtle glow effect:
```css
background: radial-gradient(ellipse 80% 50% at 50% 0%, rgba(94,106,210,0.08) 0%, transparent 70%);
```

---

## Service Catalogue

These 10 services are the canonical product list. Use these names exactly:

| Service | Slug | Related Keywords |
|---------|------|-----------------|
| AI | `ai` | Machine Learning, Automation, LLM Integration, Predictive Models, Neural Networks |
| Business Intelligence | `bi` | Data Pipelines, KPI Dashboards, Data Warehouse, Attribution, Executive Reporting |
| Web Development | `web-development` | Next.js, API Integration, CMS Architecture, E-commerce, Scalable Architecture |
| Website Support | `website-support` | Uptime SLA, Core Web Vitals, Security Patches, Monitoring, Incident Response |
| Performance Marketing | `performance-marketing` | Google Ads, Meta Ads, ROAS, A/B Testing, Conversion Rate |
| Web Analytics | `web-analytics` | GA4, Tag Management, Funnel Analysis, Attribution, Heatmaps |
| Digital Compliance | `digital-compliance` | GDPR, WCAG 2.2, ePrivacy, Cookie Consent, Audit Trail |
| Social Media Management | `social-media` | Content Strategy, Brand Voice, Community, Scheduling, Engagement |
| SEO | `seo` | Technical SEO, Schema Markup, Link Building, Search Intent, Authority |
| Creative Design | `creative-design` | Brand Identity, UI Design, Visual Systems, Typography, Motion |

---

## Imagery

- **Style:** Dark background, subtle geometric/grid overlays, glowing indigo/purple focal element
- **Service images:** WebP, stored in `/ht-assets/` — `falcon.webp`, `lynx.webp`, `mammoth.webp`, `samurai.webp`, `knight.webp`, `apache.webp`
- **Hero background:** `/ht-assets/ht-hero-bg.jpg` (fallback only — canvas is primary)
- **Badges:** SVG, stored in `/ht-assets/` — ebay, meta, iubenda, trustpilot, ringier

---

## Motion & Animation

- **Entrance animations:** Linear uses `opacity: 0 → 1` + `transform: translateY(20px) → 0` on scroll, with CSS `transition: all 0.4s ease`. Override with `animation-play-state: running` and remove `data-loaded="false"` attributes.
- **Hover transitions:** `transition: 0.15s` for color/background; `0.2s` for transforms
- **Canvas:** See [Hero Canvas doc](hero-canvas.md) for particle network specs
- **No autoplay video** — the site has a muted pulse audio asset but it is not surfaced

---

## Responsive Breakpoints

| Breakpoint | Width | Notes |
|------------|-------|-------|
| Mobile | `< 640px` | Single column, burger nav, larger touch targets |
| Tablet | `640px – 1024px` | Two column where applicable |
| Desktop | `> 1024px` | Full layout, dropdown nav visible |

Canvas hero: `height: 520px` at all breakpoints (scrollable on mobile).
