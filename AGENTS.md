# Repository Guidelines

## Project Structure & Module Organization
- `index.html`: Single‑page habit tracker app (Mavo + Google Sheets, icons via Lucide). All UI, logic, and styles live here (inline styles and utility classes).
- `style.md`: Visual language (colors, borders, typography, spacing). Treat as the canonical design guide.
- `weekly-habit-tracker-notes.md`: Reference content for habits/routines.
- `final-mavo.md`, `sheets.md`, `expressions.md` (if present): Reference docs for Mavo usage, plugins, and expressions.

## Build, Test, and Development Commands
- Local preview (recommended):
  - `python3 -m http.server 8000` then open `http://localhost:8000/index.html`.
  - Or `npx serve .` if Node is installed.
- No build step required; assets are loaded via CDN.

## Coding Style & Naming Conventions
- Language: HTML + minimal inline JS. Keep all custom CSS in `style` attributes; use utility classes on elements (not on `<html>`).
- Components: 2px borders, 2px radius, offset shadows (e.g., `box-shadow: -8px 8px 0 0 #383838`).
- Typography: Inter for body (300/400/500), Space Mono for labels; uppercase for chips/titles; tight tracking on titles >20px.
- Icons: Lucide with 1.5 stroke width. Re‑render after Mavo events.
- Mavo: Prefer declarative attributes (`mv-app`, `mv-list`, `property`, `mv-action`). Keep expressions simple and avoid naming starting with numbers.

## Testing Guidelines
- Manual flows: login gate visible when signed out; Google sign‑in works; Save writes to the target Sheet tab; toggles persist across reloads.
- Responsive checks: small (<728px), medium (~960px), large (≥1302px).
- Accessibility: visible focus outlines, sufficient contrast, keyboard toggling of buttons.

## Commit & Pull Request Guidelines
- Commits: short, imperative subject; include scope and rationale (e.g., "ui: add login overlay gate").
- PRs: describe changes, screenshots of key views, reproduction/validation steps, and any design notes referencing `style.md`.

## Security & Configuration Tips
- Google Sheets: Set `mv-storage` to the spreadsheet URL and `mv-storage-sheet="Week"`. Share the sheet with your account; do not commit secrets.
- Columns mapping (Week tab): `name, date, morningLight, workout, walk, fastingMet, guitar, stretch, journal, socialNight, notes, weekStart`.
- If headings differ, use `mv-storage-options="transformHeadings"` or update column names to match properties.

## Agent‑Specific Instructions
- Keep edits minimal and aligned with `style.md`. Do not introduce frameworks. Maintain single‑file architecture in `index.html`. Use concise diffs and explain UI changes briefly in PRs.
