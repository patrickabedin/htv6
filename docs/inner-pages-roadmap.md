# Inner Pages Roadmap

This document defines the proposed inner pages for the HT marketing site, their content structure, and how to build them using the existing design system.

---

## Page Priority

| Priority | Page | Template base | Status |
|----------|------|---------------|--------|
| 1 | `/services` | New page | Not started |
| 2 | `/services/[slug]` | 10 pages, one per service | Not started |
| 3 | `/about` | Linear's about page (`about.html`) | Not started |
| 4 | `/contact` | Linear's contact page (`contact.html`) | Not started |
| 5 | `/case-studies` | Linear's customers page (`customers.html`) | Not started |
| 6 | `/blog` | New page | Not started |

---

## Shared Layout (All Pages)

Every inner page must include:

1. **The same `<header>` block** from `index.html` (fixed nav + burger menu)
2. **The same `<style>` injection block** (CSS overrides, nav styles)
3. **The same font/CSS `<link>` tags** pointing to `/_next/static/css/`
4. **Page-specific content** in the `<main>` area
5. **Footer** — use Linear's footer from `index.html` (already content-replaced)

### Header Reuse

Copy the full `<header>...</header>` block from `index.html` verbatim into every new page. It includes:
- The HT logo
- Desktop dropdown nav
- Burger button + mobile drawer
- All inline styles + scripts

---

## Page Specs

### 1. `/services` — Services Overview

**Purpose:** Hub page listing all 10 services with brief descriptions and links to individual service pages.

**Layout:**
```
[Hero]
  H1: "Our Services"
  Subtext: brief intro (1–2 sentences)

[10-card grid]
  Each card:
    - Service icon (from existing SVGs in carousel pills)
    - Service name
    - 2-sentence description
    - "Learn more →" link to /services/[slug]

[Footer CTA]
  Same as homepage
```

**Reuse:** The 10 service SVG icons are already defined in `index.html`'s carousel section — extract them.

---

### 2. `/services/[slug]` — Individual Service Pages

One page per service. 10 total. Proposed structure:

```
[Hero]
  H1: [Service Name]
  Subtext: 1–2 sentence value proposition
  CTA: "Request a Proposal" → hellenictechnologies.com/contact-us/

[What We Do]
  3-column feature grid
  Each column: icon + heading + 2-sentence description

[How It Works]
  Numbered steps (3–4 steps)
  Linear's existing step/timeline CSS classes work well here

[Who It's For]
  Target audience section
  2–3 persona cards (e.g. "E-commerce brands", "Enterprise IT teams")

[Results / Outcomes]
  3 stat callouts (use Linear's stat component CSS)
  e.g. "43% average ROAS improvement", "99.9% uptime SLA"

[Footer CTA]
  Same as homepage
```

**Service slugs and pages:**

| Slug | Page path | External link |
|------|-----------|---------------|
| `web-development` | `/services/web-development` | `hellenictechnologies.com/services/site-as-you-go-falcon/` |
| `website-support` | `/services/website-support` | `hellenictechnologies.com/services/managed-wordpress-maintenance/` |
| `performance-marketing` | `/services/performance-marketing` | `hellenictechnologies.com/services/digital-as-you-go-samurai/` |
| `seo` | `/services/seo` | `hellenictechnologies.com/services/seo-as-you-go-viking/` |
| `social-media` | `/services/social-media` | `hellenictechnologies.com/services/social-as-you-go-knight/` |
| `creative-design` | `/services/creative-design` | `hellenictechnologies.com/services/design-as-you-go-apache/` |
| `ai` | `/services/ai` | TBD |
| `business-intelligence` | `/services/business-intelligence` | TBD |
| `web-analytics` | `/services/web-analytics` | TBD |
| `digital-compliance` | `/services/digital-compliance` | TBD |

---

### 3. `/about` — About Hellenic Technologies

**Base:** `about.html` from the Linear mirror (already in repo)

**Key replacements needed:**
- Company name, founding story, mission statement
- Team section → HT team or remove
- Investor logos → Partner/client logos (ebay, meta, iubenda, trustpilot, ringier badges already in `/ht-assets/`)
- Values section → HT values (Precision, Reliability, Scale, Clarity)
- Stats: years operating, clients, countries, projects delivered

---

### 4. `/contact` — Contact

**Base:** `contact.html` from the Linear mirror

**Key replacements needed:**
- Form endpoint → hellenictechnologies.com contact form or Formspree
- Office location: Athens, Greece
- Email: relevant HT email
- Remove Linear-specific options (API, Enterprise tiers)
- Keep the clean form layout — it's good

---

### 5. `/case-studies` — Client Work

**Base:** `customers.html` from the Linear mirror

**Structure:**
- Grid of client case study cards
- Each card: client logo, industry, headline result, link to full case study
- If no case studies exist yet: use as a "clients" / logo wall page instead

---

## Building Inner Pages — Step by Step

1. **Copy `index.html`** as the base for the new page
2. **Strip the homepage-specific sections** (hero canvas, carousel, demo panel, testimonials)
3. **Keep:** `<head>`, `<header>`, injected `<style>`, fonts, footer, closing scripts
4. **Insert page content** in `<main>` using Linear's existing CSS classes where possible
5. **Add to `vercel.json`** — the current SPA rewrite handles all routes already, no change needed
6. **Add to nav** — update the dropdown in the shared header block

---

## CSS Classes to Reuse from Linear

These are safe to use in new pages — they're in the unmodified CSS chunks:

| Class pattern | Visual component |
|---------------|-----------------|
| `sc-d5151d0-0 kEiGEn` | H3 heading style |
| `sc-d5151d0-0 eBXKXP` | Body paragraph style |
| `Benefits_carouselCard__Fjt3f` | Pill/feature cards |
| `Carousel_inner__L4Uvf` | Horizontal scroll container |
| `Hero_container` | Hero section wrapper |
| `Frame_wrapper___hKDg` | Visual panel container |

To discover more: inspect `_next/static/css/` chunks, or use browser DevTools on `index.html`.

---

## Content Tone Reminder

See [Design System](design-system.md#copy-rules) for the full copy rules. Short version:

- No hyphens, no exclamation marks, no em dashes
- Enterprise verbs: deploy, engineer, optimise, accelerate
- Direct, declarative, confident — not enthusiastic or startup-y
- Every page should feel like it belongs to a premium agency, not a SaaS product

---

## Domain / Routing

Currently: `https://ht3-hellenic.vercel.app`  
Future: map a custom domain (e.g. `ht3.hellenicai.com`) via Cloudflare + Vercel once inner pages are live.

Cloudflare zone: `hellenicai.com` (ID: `f16097e2e26afe339fd913931018f4f5`)
