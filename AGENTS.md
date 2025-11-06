# Repository Guidelines

## Project Structure & Module Organization
- `index.html` (and variants like `index7.html`): Single‑page habit tracker. All UI, logic, and styles live in the file (inline styles + utility classes). Newer variants use Firebase Auth/DB, Chart.js, Lucide, and optional Lottie web components.
- `style.md`: Visual language (colors, borders, typography, spacing). Treat as the canonical design guide.
- `weekly-habit-tracker-notes.md`: Source content for “tips” carousel.
- `final-mavo.md`, `sheets.md`, `expressions.md` (if present): Legacy Mavo/Sheets references.

## Build, Test, and Development Commands
- Local preview (recommended):
  - `python3 -m http.server 8000` then open `http://localhost:8000/index.html`.
  - Or `npx serve .` if Node is installed.
- No build step required; assets are loaded via CDN.

Deployment note
- Host at `https://habits.roomtolearn.org/`. Ensure the canonical link, Open Graph, Twitter, and JSON‑LD tags are present (see SEO section below). Update `og-image.png` at the root URL.

## Coding Style & Naming Conventions
- Language: HTML + minimal inline JS. Keep all custom CSS inline via `style` attributes and small utility classes (no frameworks beyond CDNs already included).
- Components: 2px borders, 2px radius, offset shadows (e.g., `box-shadow: -8px 8px 0 0 #383838`).
- Typography: Inter for body (300/400/500), Space Mono for labels; uppercase for chips/titles; tight tracking on titles >20px.
- Icons: Lucide with 1.5 stroke width. Re‑render after dynamic DOM changes.
- Tips: Copy comes from `weekly-habit-tracker-notes.md`. Carousel cycles every 4s with a subtle fade.
- Animations: Side Lottie “drifters” only (dotlottie‑wc). They stay off the content column, drift down via CSS, and shift horizontally with scroll. Hidden below 728px.
- Legacy Mavo: Prefer declarative attributes (`mv-app`, `mv-list`, `property`, `mv-action`) in legacy files only.

## Authentication & Demo Mode (Firebase)
- Auth: Google OAuth (Firebase). Remove email/password flows.
- Signed out: Show read‑only analytics using demo UID `HkjWermUkCdEMbTJxSswSBSGCap2`. Editing is disabled.
- Signed in: Show full editing UI; writes go to the user’s UID.
- UI: Header shows “Sign in with Google” when signed out; user email and “Sign Out” when signed in.

## Data Storage Model (Compact Yearly + Legacy)
- Legacy path (compat): `habitsTracker/<uid>` stores per‑day objects.
- Compact path (preferred): `habitsY/<uid>/<year>` with shape `{ h, n, v }`:
  - `h`: Base64 bitset of size `daysInYear * habitsCount` (row‑major: day, then habit). Bit is 1 if habit completed.
  - `n`: Sparse notes map `{ [dayOfYear]: string }`.
  - `v`: Version number (start at 1).
- Habits enum: The index of each habit in `habitsConfig` defines its bit position. Never reorder existing items; only append new habits to maintain compatibility.
- Size target: Entire year well under 10 MB. Bitset is ~0.6 KB for 12 habits; notes dominate size—keep them reasonably short.
- Load strategy: Prefer `habitsY`. If missing, fall back to `habitsTracker`.
- Save strategy: When signed in, write both formats to preserve compatibility.

## SEO
- Required tags in `<head>`: canonical to `https://habits.roomtolearn.org/`, description, robots, Open Graph (url, title, description, image, image:alt), Twitter (summary_large_image), theme‑color, and JSON‑LD `WebSite` schema.
- Provide `og-image.png` at `https://habits.roomtolearn.org/og-image.png`.

## UX Guidelines
- Stats tiles: 2px bordered cards with label chips; no gradients.
- Checkboxes: crisp 2px boxes, small “pop” animation on toggle.
- Read‑only indicators: Disable controls when signed out; keep charts visible.

## Testing Guidelines
- Manual flows: signed‑out shows demo analytics (no edits); Google sign‑in works; saving writes both legacy and compact; toggles persist across reloads.
- Responsive checks: small (<728px), medium (~960px), large (≥1302px).
- Accessibility: visible focus outlines, sufficient contrast, keyboard toggling of buttons.

## Commit & Pull Request Guidelines
- Commits: short, imperative subject; include scope and rationale (e.g., "ui: add login overlay gate").
- PRs: describe changes, screenshots of key views, reproduction/validation steps, and any design notes referencing `style.md`.

## Security & Configuration Tips
- Firebase: Do not commit secrets outside of the public client config. Database rules should restrict writes to `auth.uid == $uid` and block writes for the demo UID.
- If any legacy Google Sheets integration remains, keep sheet URLs out of version control.

## Agent‑Specific Instructions
- Keep edits minimal and aligned with `style.md`. Maintain single‑file architecture. Use concise diffs and explain UI changes briefly in PRs.
- When adding habits, append to `habitsConfig` (don’t reorder) to preserve bit positions in compact storage.
- When changing animations, keep Lottie elements out of the content column and respect the 2px visual language.
