# 005 — 2px paper-card ("neobrutalist") design language

- **Date:** 2025-11-04 → 2025-11-05
- **Commits:** `b1432db` (`style.md` authored), `ea9d0de` (shared component classes), `f20d700` (animation layer)
- **Status:** accepted — canonical, codified in `style.md`

## Context

The repo's second file ever was `style.md`, a design system written before
the app: playful, editorial, paper-like. `todo.html` proved the language
interactively. Every variant since conforms.

## Decision

Adopt and enforce the `style.md` visual language:

- **Colors:** ink `#383838` on paper `#F4EFEA`, white cards, yellow
  `#FFDE00` highlight, blue `#6FC2FF` primary CTA, focus `#2BA5FF`, muted
  `#A1A1A1`; extended accents (salmon, teal, purple, lime, gold) for tiles.
- **Geometry:** 2px borders everywhere, 2px radius, offset hard shadows
  (`-8px 8px 0 0 #383838`), no gradients.
- **Type:** Inter 300/400/500 for body; Space Mono for labels/chips;
  uppercase titles with chips instead of bold weights; tight tracking on
  titles >20px.
- **Icons:** Lucide at 1.5 stroke width; no gradient icon containers.
- **Motion:** CSS-only. Hover = small translate (buttons "lift" toward their
  shadow); checkbox "pop" on toggle (restarted via forced reflow);
  decorative Lottie drifters drift down side lanes and shift horizontally
  with scroll (rAF-throttled, passive listener), hidden below 728px.

## Consequences

- `ea9d0de` consolidated one-off Tailwind utilities into shared classes
  (`.btn`, `.card`, `.input`, `.link`, `.label-chip`) — later changes stay
  consistent because the classes encode the tokens.
- Charts (Chart.js) required custom color/legend work to sit well in the
  light paper palette; canvases must be wrapped in `<div>`s to avoid the
  Chart.js infinite-resize bug.
- The strong visual identity is the app's most recognizable feature; any UI
  contribution that breaks the 2px language sticks out immediately.
