# Repository Guidelines

Guidance for humans and AI agents working in this repository. See also
`README.md` (overview), `CHANGELOG.md` (per-commit history), and
`architectural-diary/` (decision records).

## Project Structure & Module Organization

- `index.html` — **the production app.** Single-page habit tracker; all UI,
  logic, and styles live in this one file (inline `<style>` + Tailwind CDN +
  Firebase compat SDK + Chart.js + Lucide + dotlottie-wc). Built around
  `habitsConfig` (12 habits), `habitsData` (in-memory model), and a render
  pipeline (`renderAllViews` → stats/charts/recent/logbook).
- Prototype variants, kept for reference only: `index2.html`–`index5.html`
  (Mavo-based), `index6.html`/`index8.html` (Firebase experiments; `index8`
  is a dark dashboard on the v10 modular SDK with a hex-bitmask format),
  `oldindex.html` (the pre-Firebase Mavo/Sheets page, archived verbatim), and
  `todo.html` (standalone todo prototype that seeded the design language).
- Data/seed files: `habits.json`, `habitTracker.json`, `habitTracker5.json`,
  `init2.csv`, `init3.csv`.
- Docs: `style.md` (canonical visual language), `weekly-habit-tracker-notes.md`
  (circadian source content + tips copy), `goal.md` (original build prompt),
  `final-mavo.md` / `mavo-api.md` / `mavo-expressions.md` / `style-mavo.md` /
  `expressions.md` / `sheets.md` (legacy Mavo/Sheets references).

### Architecture map (index.html)

- Auth: `initAuth`, `signInWithGoogle`, `signOut`, `updateAuthUI`.
  `DEFAULT_DEMO_UID` supplies read-only data when signed out.
- Dates: `getDayKey`/`getDateFromKey` (`YY-DDD`, e.g. `25-308`),
  `daysInYear`, `isLeapYear`. `LOGBOOK_MIN_DATE` (Nov 4, 2025) anchors all
  statistics.
- Storage: `encodeYearCompact`/`decodeYearCompact` (bitset ↔ `habitsData`),
  `loadHabits` (prefers `habitsY`, falls back to `habitsTracker`),
  `saveHabits`/`autoSave` (writes both paths when signed in).
- Views: `renderAllViews` → `updateStats`, `renderCharts`
  (`renderWeeklyChart`, `renderHabitRadarChart`, `renderMonthlyTrendChart`,
  `renderHabitMatrixChart`), `renderRecentHabits`, `renderLogbook`,
  `startTipsCarousel`, `initDrifters`, `initScrollParallax`.
- Mutations: `toggleHabit` (bitset + notifications), `updateNotes`,
  `changeRecentPage`, `changeLogbookWeek`.
- Notifications: `queueNotification`/`processNotificationQueue` (serialized,
  encouraging messages on habit toggles).

## Commands

- Local preview: `python3 -m http.server 8000` → open
  `http://localhost:8000/index.html` (or `npx serve .`). No build, no install,
  no test suite — everything is CDN-loaded and manual.
- Deployment: host the repo root at `https://habits.roomtolearn.org/` (static
  hosting). Keep canonical/OG/Twitter/JSON-LD tags pointing there; provide
  `og-image.png` at the site root.
- Git: short imperative subjects with a conventional-commit prefix
  (e.g., `feat:`, `fix:`, `chore(data):`); body explains the why/what.

## Conventions

- Keep the single-file architecture. Custom CSS stays in the inline
  `<style>` block / utility classes; no frameworks beyond the CDNs included.
- Visual language (from `style.md`): 2px borders, 2px radius, offset shadows
  (`box-shadow: -8px 8px 0 0 #383838`), ink `#383838` on paper `#F4EFEA`,
  yellow `#FFDE00` / blue `#6FC2FF` accents; Inter body (300/400/500) + Space
  Mono labels; uppercase chips/titles; no gradients; no bold headings.
- Icons: Lucide, 1.5 stroke width. Re-run `lucide.createIcons()` after any
  dynamic DOM injection.
- Charts: Chart.js canvases must be wrapped in a `<div>` (a bare canvas
  sibling of text nodes triggers an infinite-resize bug).
- Animations: CSS only (never JS-driven). Lottie "drifters" stay outside the
  content column and are hidden below 728px.

## Gotchas & invariants

- **Never reorder `habitsConfig`.** Bit positions in the compact bitset are
  the array index of each habit; reordering corrupts every stored year. Only
  append new habits.
- Compact storage (`habitsY/<uid>/<year>` = `{ h: base64 bitset, n: sparse
  notes by day-of-year, v: 1 }`) must stay well under 10 MB/year — it is
  ~0.6 KB for 12 habits; notes dominate, keep them short.
- Always write **both** storage paths on save (`habitsY` + legacy
  `habitsTracker`) and prefer `habitsY` on load.
- All statistics must respect `LOGBOOK_MIN_DATE`; days before it do not exist
  for streaks, averages, totals, or charts.
- Signed-out users see read-only demo analytics (`DEFAULT_DEMO_UID`); editing
  controls must be disabled. Database rules should enforce
  `auth.uid == $uid` and block writes to the demo UID.
- No secrets in the repo. The Firebase web config is public client
  configuration; real secrets (service accounts, admin keys) never go in.
- Mavo files (`index2`–`index5`, `oldindex`) are legacy — prefer declarative
  `mv-*` attributes there and do not port their patterns into `index.html`.

## Verifying changes

There are no automated tests. Verify manually:
1. Load `index.html` signed out: demo analytics render, controls disabled.
2. Sign in with Google: checkboxes toggle with a pop + notification; edits
   autosave (debounced) to both `habitsY` and `habitsTracker`.
3. Reload: data persists; streaks/monthly average/total days match
   `LOGBOOK_MIN_DATE` anchoring.
4. Paginate Recent Progress and Logbook: buttons disable at range bounds.
5. Responsive checks at <728px (drifters hidden), ~960px, ≥1302px.
6. Accessibility: visible focus rings, keyboard-operable buttons/checkboxes.

## Agent-specific instructions

- Keep edits minimal and aligned with `style.md`; explain UI changes briefly.
- Append-only `habitsConfig`; preserve trailer lines (Co-Authored-By, etc.)
  in commit messages; do not rewrite history without a local backup branch.
- When touching analytics, keep the `LOGBOOK_MIN_DATE` exclusions consistent
  across streaks, averages, totals, and all four charts.
