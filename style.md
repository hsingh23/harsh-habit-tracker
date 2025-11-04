# Style Guide

This document summarizes the visual language implemented in `duck.html` and the provided CSS excerpt. It is a concise but practical reference you can hand to designers and engineers when extending the site.

## Overview

- Tone: playful, editorial, and product‑centric. Visuals mimic paper cards with bold 2px outlines and offset “drop” shadows.
- Layout: generous negative space, big uppercase headlines, grid/card compositions, and simple shapes (clouds) as accents.
- Interaction: minimal motion; hover primarily uses small translations and thicker underlines.
- Accessibility: high‑contrast borders and clear focus rings; body text uses Inter at light weights for readability.

## Color Palette

Core brand colors (hex from CSS rgb values):

- Ink: `#383838`
- Background (paper): `#F4EFEA`
- White: `#FFFFFF`
- Yellow (highlight/eyebrow): `#FFDE00`
- Blue (primary CTA): `#6FC2FF`
- Focus/outline blue: `#2BA5FF`
- Muted gray: `#A1A1A1`

Extended accents seen in tiles and badges:

- Salmon: `#FF7169`
- Teal: `#53DBC9`
- Purple: `#B291DE`
- Lime: `#B3C419`
- Gold: `#E1C427`
- Steel blue: `#84A6BC`
- Periwinkle: `#7597EE`

Suggested CSS variables

```css
:root {
  --bg: #f4efea;
  --ink: #383838;
  --card: #ffffff;
  --yellow: #ffde00;
  --blue: #6fc2ff;
  --focus: #2ba5ff;
  --muted: #a1a1a1;
  --radius: 2px; /* consistent across the UI */
  --border-w: 2px; /* heavy outline */
}
```

## Typography

Fonts and pairing:

- Headings: Aeonik Mono (project CSS) or Space Mono fallback; uppercase, normal weight (400).
- Body: Inter, weights 300 and 400.
- Buttons/links: Inter 400, uppercase for CTAs; link text retains sentence case but uses underlined spans.

Recommended sizes and responsive behavior (from CSS and `duck.html`):

- h1: 30 → 56 → 80 → 96 px (mobile → tablet → desktop → wide); line-height ~120%; uppercase.
- h2: 24 → 32 → 40 px; uppercase.
- h3: 24 → 32 px; uppercase.
- Body: 16 px (20 px on larger viewports for long‑form copy).
- Label/small: 14 px.
- Letter‑spacing: `0.02em` for most text; headlines may keep default tracking.

Usage notes:

- Headlines and section labels use background chips (dark background, white text) or bordered chips (white background, 2px border).
- Avoid bold weight for headings; leverage size/uppercase + chips for emphasis.

## Spacing System

Spacing uses an 8‑pt base with some 4‑pt steps. Common tokens:

- 4, 8, 12, 16, 20, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 120, 140, 160, 180, 200.
  Examples from the layout: section paddings 64–200, gaps 16/24/32/40/64, header heights 55/70/90.

## Border Radius

- Global radius: `2px` on buttons, inputs, cards, checkboxes. Keep corners crisp.

## Borders

- Standard outline: `2px solid var(--ink)` on cards, buttons, inputs.
- Focus state: `outline-color: var(--focus)` or `border-color: var(--focus)` on hover/focus.

## Shadows & Elevation

Instead of soft shadows, the design uses offset, flat “paper” shadows:

- Card shadow: `box-shadow: -8px 8px 0 0 var(--ink);` (variants `-5px`, `-6px`, `-12px` used responsively).
- Optional glass overlay on imagery: `::after { background: rgba(56,56,56,.18); }`.

## Animations & Transitions

- Buttons/cards: translate on hover `transform: translate(6–7px, -6–7px);` with `transition: 120ms`.
- Press/active: reset transform instantly (~30ms) to feel “clicky”.
- Feature tiles: scale up to 1.1 on hover in 300ms for a playful feel.
- Link underlines: border‑bottom thickness increases on hover.

## Opacity & Transparency

- Inputs: `background: rgba(248,248,247,0.7)` for a vellum feel.
- Disabled buttons: gray text and `background: #F8F8F7` with muted border.

## Breakpoints

Custom breakpoints from the CSS excerpt:

- `728px` (tablet)
- `960px` (desktop)
- `1302px` (wide)
- `1600px` (x‑wide enhancements)

## Component Styles

Buttons

```css
.btn {
  border: var(--border-w) solid var(--ink);
  border-radius: var(--radius);
  background: var(--bg);
  color: var(--ink);
  text-transform: uppercase;
  font: 400 16px/1.2 Inter, sans-serif;
  padding: 16.5px 22px;
  transition: transform 120ms ease-in-out;
}
.btn:hover {
  transform: translate(7px, -7px);
}
.btn:active {
  transform: none;
}
.btn.primary {
  background: var(--blue);
}
```

Link (underline accent)

```css
.link {
  font: 400 16px/1.2 Inter, sans-serif;
}
.link span {
  border-bottom: 0.12em solid var(--ink);
  padding: 1px 0;
}
.link:hover span {
  border-bottom-width: calc(0.12em + 1px);
}
```

Card with offset shadow

```css
.card {
  background: #fff;
  border: var(--border-w) solid var(--ink);
  box-shadow: -8px 8px 0 0 var(--ink);
}
```

Input

```css
.input {
  background: rgba(248, 248, 247, 0.7);
  border: var(--border-w) solid var(--ink);
  border-radius: var(--radius);
  font: 400 16px Inter, sans-serif;
  padding: 16px 16px;
}
.input:hover,
.input:focus {
  border-color: var(--focus);
  outline: none;
}
```

“Chip” section label

```css
.label-chip {
  font: 400 18px/1.4 "Space Mono", "Aeonik Mono", monospace;
  text-transform: uppercase;
  background: var(--ink);
  color: #fff;
  padding: 8px 12px;
}
```

## Example Component Reference (HTML)

```html
<a class="btn primary" href="#">Start Free</a>
<a class="link" href="#"><span>Learn more</span></a>

<div class="card" style="padding:24px; max-width:640px;">
  <div class="label-chip">Finally:</div>
  <p style="font-family:Inter; font-weight:300;">A database you don’t hate.</p>
</div>

<input class="input" placeholder="Work email" />
```

## Tailwind CSS Mapping (if migrating)

The project does not use Tailwind, but these are sensible mappings:

- Colors: define in `theme.extend.colors`:

```js
colors:{
  ink: '#383838', paper:'#F4EFEA', brand:'#6FC2FF', focus:'#2BA5FF', yellow:'#FFDE00'
}
```

- Radius: `rounded-[2px]`
- Borders: `border-2 border-ink`
- Buttons: `uppercase font-sans text-[16px] py-[16.5px] px-[22px] bg-brand hover:translate-x-[7px] hover:-translate-y-[7px] transition-transform`
- Card shadow: custom in `theme.extend.boxShadow`:

```js
boxShadow: {
  offset: "-8px 8px 0 0 #383838";
}
```

- Breakpoints:

```js
screens:{ tb:'728px', md:'960px', lg:'1302px', xl:'1600px' }
```

## Assets & Iconography

- Simple geometric shapes (clouds) drawn in inline SVG with 2px stroke in `#383838` and white fill.
- Screenshots/illustrations often receive a subtle overlay `rgba(56,56,56,.18)`.

## Content Guidelines

- Use sentence‑case body copy; reserve ALL CAPS for display/labels.
- Keep CTAs short (2–3 words). Primary CTA uses the blue fill; secondary uses paper background with outline.

## Do & Don’t

- Do keep borders 2px for consistency.
- Do use offset shadows instead of blurred ones.
- Don’t overuse bold weights; rely on chips, size, and color.
- Don’t mix soft box‑shadows with the offset style on the same surface.

## Navigation Patterns

- Desktop: top nav shows logo, section links, and a CTA button. Links use Inter 14px; CTA uses the blue fill. Keep a 2px border under the entire header only when sections change.
- Mobile: use a hamburger icon (three 2px bars). Tapping opens a full‑screen overlay with vertical links, a close “×” icon in the top right, and the primary CTA at the bottom of the list.

Reference CSS

```css
.hamburger {
  width: 28px;
  height: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.hamburger span {
  height: 2px;
  background: var(--ink);
}
.overlay {
  position: fixed;
  inset: 0;
  background: var(--bg);
  display: none;
  padding: 24px;
  z-index: 10010;
  overflow-y: auto;
}
.overlay.open {
  display: block;
}
.overlay nav {
  display: flex;
  flex-direction: column;
  gap: 24px;
  margin-top: 24px;
}
body.no-scroll {
  overflow: hidden;
}
@media (min-width: 960px) {
  .menu {
    display: flex;
  }
  .hamburger {
    display: none;
  }
}
```

## “Finally” Strip

- A dark label chip on the left with white uppercase text followed by a short sentence on the right.
- Use the `.label-chip` token in this guide; place on a card border to emphasize the breakpoint between sections.

## Features Grid

- Two or more bordered cards with offset shadow and a media area (16/9) followed by a caption.

Reference CSS

```css
.features {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}
@media (min-width: 728px) {
  .features {
    grid-template-columns: 1fr 1fr;
  }
}
.features .card {
  padding: 0;
}
.features .media {
  aspect-ratio: 16/9;
  border-bottom: var(--border-w) solid var(--ink);
  display: flex;
  align-items: center;
  justify-content: center;
}
```

## Buttons – Hover Shadow

Two-layer card effect on hover (using box-shadow layers).

- Idle: flat button with 2px outline.
- Hover/focus: button shifts `translate(6px,-6px)` and gets two crisp offset layers via multi-value `box-shadow`.
- Active: transform resets and shadows are cleared.

Reference

```css
.btn {
  background: var(--blue);
  border: 2px solid var(--ink);
  border-radius: 2px;
  transition: transform 0.18s cubic-bezier(0.22, 0.61, 0.36, 1), box-shadow
      0.18s cubic-bezier(0.22, 0.61, 0.36, 1);
}
.btn:hover,
.btn:focus-visible {
  transform: translate(6px, -6px);
  box-shadow: -6px 6px 0 0 var(--ink), -12px 12px 0 0 var(--ink);
}
.btn:active {
  transform: none;
  box-shadow: none;
}
```

## Decorative Asset Placement via JSON

- Hero clouds and similar doodles are positioned responsively using a JSON config embedded in the HTML.
- Schema:

```json
{
  "breakpoints": [
    {
      "min": 0,
      "assets": {
        "cloud-left": { "x": "1.5%", "y": "18%", "w": 56, "z": 0, "py": -0.06 },
        "cloud-top-right": { "right": "6%", "y": "16%", "w": 28, "py": -0.1 },
        "cloud-bottom-right": {
          "right": "0%",
          "y": "74%",
          "w": 200,
          "z": -1,
          "py": 0.12
        }
      }
    },
    {
      "min": 960,
      "assets": { "cloud-left": { "x": "2%", "y": "4%", "w": 72 } }
    }
  ]
}
```

- Runtime logic selects the highest `min` ≤ viewport width, then applies `left`/`right` (`x`/`right`), `top` (`y`), `width` (`w` px), and `z` to elements matching `[data-asset]`.
- New: `py` is a parallax speed factor for vertical movement. On scroll, the element moves by `scrollY * py` pixels via `transform: translateY(...)`. Negative values move against the scroll for a subtle depth effect.

## Parallax Guidance

- Keep parallax subtle: `|py|` in the range `0.04–0.14` feels close to the reference site.
- Use fewer layers; 2–3 moving elements are enough. Avoid parallax on interactive elements.
- All parallax updates run inside `requestAnimationFrame` and use transforms to stay performant.

Add a new parallax asset

1. Place an element with `data-asset="name"`.
2. Add positions in the JSON for each breakpoint and include `"py": <speed>` if it should parallax.

## Use‑Cases Banner (scroll‑linked marquee)

- A blue strip with repeated text scrolls horizontally as the page scrolls.
- Markup: a `.usecases` section containing `.ticker` with repeated spans.
- Behavior: add `data-scroll-x="-0.35"` (negative to move left as you scroll down). JS maps vertical scroll to horizontal translate.

Reference CSS

```css
.usecases {
  border-top: 2px solid var(--ink);
  border-bottom: 2px solid var(--ink);
  background: var(--blue);
  padding: 16px 0;
  overflow: hidden;
}
.usecases .ticker {
  white-space: nowrap;
  display: inline-flex;
  gap: 120px;
  will-change: transform;
}
.usecases .ticker span {
  font-family: "Space Mono";
  font-size: 40px;
  letter-spacing: 0.04em;
  color: var(--ink);
}
```

- Add a new asset by placing an element with `data-asset="name"` and adding a matching entry in the JSON.

---

This guide should be sufficient to reproduce the look/feel across new pages and components without digging through the raw CSS.

## Hero Details

- Kicker: `DUCKDB CLOUD DATA WAREHOUSE` in Space Mono 16px, uppercase.
- Headline: Space Mono 48 → 80 → 96 px; line-height 120%; uppercase.
- Subhead: Inter 16px, uppercase with `letter-spacing: 0.08em`; class `hero-sub`.
- Screenshot card adds a bottom “base” using `::before` to mimic the real page’s stand.

Reference

```css
.shot {
  position: relative;
  box-shadow: -12px 12px 0 0 var(--ink);
}
.shot::before {
  content: "";
  position: absolute;
  left: 12px;
  right: 12px;
  bottom: -10px;
  height: 12px;
  background: var(--ink);
  border-radius: 2px;
}
```

## Decorative Assets JSON (updated positions)

Tweaked cloud placement to better match production. See `duck.html` script `#asset-positions` for the new values — notably larger bottom-right cloud and left cloud lower in the hero.
