# Hero Canvas — Particle Network

The hero visual is a self-contained `<canvas>` element injected inside `.Frame_wrapper___hKDg`, replacing Linear's screenshot imagery. No external dependencies — pure Canvas 2D API + vanilla JS.

---

## Visual Design Intent

A live network of floating keyword nodes connected by thin luminous edges, with signal pulses traveling between them. The aesthetic is:

- **Dark, restrained** — `#07070d` background, single indigo/slate color family
- **Connections as the star** — edges are the primary visual element; dots are secondary
- **Alive but not distracting** — slow drift, subtle pulse, occasional signal flash
- Directly metaphoric — an interconnected service network

---

## Color Spec

```js
var DIM  = "148,163,184";  // slate-400 — base nodes + edges
var GLOW = "165,180,252";  // indigo-300 — accent nodes + signal trails
// White: rgba(255,255,255,*) — signal core dots + specular
```

**Do not add more colors.** The restraint is intentional.

---

## Keywords

34 nodes, one per keyword. Distributed across all 10 service areas:

```
AI:                  Machine Learning, Automation, LLM Integration, Predictive Models
BI:                  Data Pipelines, KPI Dashboards, Data Warehouse, Attribution
Web Dev:             Next.js, API Integration, CMS Architecture, E-commerce
Website Support:     Uptime SLA, Core Web Vitals, Security Patches
Perf. Marketing:     Google Ads, Meta Ads, ROAS, A/B Testing
Web Analytics:       GA4, Tag Management, Funnel Analysis
Compliance:          GDPR, WCAG 2.2, ePrivacy
Social Media:        Content Strategy, Brand Voice, Community
SEO:                 Technical SEO, Schema Markup, Link Building
Creative Design:     Brand Identity, UI Design, Visual Systems
```

To add/change keywords: edit the `KEYWORDS` array in the canvas script block in `index.html`.

---

## Physics Parameters

```js
// Node movement
vx/vy initial:  rand(-0.15, 0.15)  // gentle starting velocity
max speed:      0.6 px/frame
damping:        0.96 per frame (×each tick)
min speed:      0.05 → random nudge applied below this
centre gravity: 0.00008 (keeps nodes from flying off screen)

// Mouse / touch repulsion
radius:         200px
strength:       pow((200-dist)/200, 2) × 0.18
(quadratic falloff — snappy near cursor, zero at 200px)

// Node-node repulsion
radius:         60px
strength:       (60-dist)/60 × 0.05
(prevents clumping)

// Edge connection distance
CONNECT_DIST:   min(W, H) × 0.32
```

---

## Edge Rendering

```js
ctx.strokeStyle = "rgba(148,163,184," + (1 - dist/CONNECT_DIST) * 0.32 + ")";
ctx.lineWidth   = 0.8;
```

Opacity falls off linearly with distance. Maximum alpha is `0.32` (at distance 0).

---

## Signal Pulses

Signals spawn randomly each frame (`probability: 0.025` if fewer than 12 active):

```js
// Spawn: pick two random nodes within CONNECT_DIST of each other
// Travel: lerp from node A to node B over ~125–250 frames (speed 0.004–0.008/frame)
// Visual: radial gradient glow (radius 7, alpha 0.75) + solid white dot (radius 2)
```

---

## Node Rendering

```
r (radius):   rand(2.2, 3.4) + sin(pulse) × 0.6  — small clean dots
bright nodes: 35% of nodes (Math.random() > 0.65)

Halo:         radial gradient, radius pr×8
              bright: rgba(GLOW, 0.15) → transparent
              base:   rgba(DIM,  0.07) → transparent

Core fill:    bright: rgba(165,180,252, 0.9)
              base:   rgba(148,163,184, 0.75)
```

---

## DPR (Retina / Mobile)

```js
DPR = Math.min(window.devicePixelRatio || 1, 2);
canvas.width  = W * DPR;
canvas.height = H * DPR;
ctx.setTransform(DPR, 0, 0, DPR, 0, 0);
// All drawing coordinates remain in CSS pixels — DPR is transparent to physics
```

---

## Responsive Labels

```js
var mobile    = W < 640;
var fontSize  = mobile ? 11 : 10;      // px
var fontWeight= mobile ? "500" : "400";
var alpha     = mobile ? 0.65–0.72 : 0.28–0.32;
```

Bright nodes use `rgba(165,180,252, alpha)`, base nodes use `rgba(203,213,225, alpha)`.

---

## Resize Handling

Mobile Chrome fires `window.resize` when the address bar shows/hides during scroll. This was causing node position resets.

**Fix — only reinitialise when width changes:**

```js
var lastW = 0, resizeTimer;
window.addEventListener("resize", function() {
  clearTimeout(resizeTimer);
  resizeTimer = setTimeout(function() {
    var newW = canvas.parentElement.getBoundingClientRect().width || 900;
    if (Math.abs(newW - lastW) < 10) return; // height-only change — skip
    lastW = newW;
    cancelAnimationFrame(raf);
    applySize();
    initNodes();
    tick();
  }, 150);
});
```

---

## Touch Input

```js
// Touch events are passive — no preventDefault
// This allows native page scroll to work unobstructed
canvas.addEventListener("touchmove", function(e) {
  var r = canvas.getBoundingClientRect(), t = e.touches[0];
  mouse.x = (t.clientX - r.left) * (W / r.width);
  mouse.y = (t.clientY - r.top)  * (H / r.height);
});
// Coordinates scaled by (W / r.width) to account for canvas CSS vs logical size
```

---

## Tuning Cheat Sheet

| Want to... | Change |
|------------|--------|
| More nodes / keywords | Add to `KEYWORDS` array |
| Faster movement | Increase initial `vx/vy` range, raise max speed |
| More connections | Increase `CONNECT_DIST` multiplier (e.g. `0.4`) |
| Brighter edges | Increase `0.32` alpha multiplier on edges |
| More signals | Increase spawn probability (`0.025`) or max active (`12`) |
| Bigger dots | Increase `rand(2.2, 3.4)` range |
| Stronger mouse repulsion | Increase `0.18` multiplier |
| Slower pulse | Decrease `0.015` per tick increment |
