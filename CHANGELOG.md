# Changelog

All notable changes to this project are documented here, newest first.

> **History rewrite note (2026-09-08):** On 2026-09-08 the commit messages on
> `main` were improved via a messages-only history rewrite (`git filter-branch
> --msg-filter`). File contents and the commit tree are byte-identical to the
> previous history; only commit messages (and therefore hashes) changed.
> Auto-generated Mavo messages ("Created file", "Updated habitTracker.json")
> were replaced with descriptive conventional-commit messages. A local backup
> branch `backup/pre-docs-20260908` preserves the pre-rewrite history. Hashes
> below reflect the rewritten history.

## 2025-11-06

### `0dbefb9` — feat: add pagination & week navigation for Recent tracker and Logbook
- Add prev/next pagination with range labels to Recent Progress (3-day pages, `recentOffsetDays`) and Logbook (weekly pages, `logbookWeekOffset`), both clamped to `LOGBOOK_MIN_DATE` (Nov 4, 2025); nav buttons disable when out of range.
- Rework Logbook rendering to show one Sun–Sat week at a time, hide empty days, and fully escape note HTML.
- Exclude dates before `LOGBOOK_MIN_DATE` from all analytics (streaks, monthly average, totalDays, weekly chart, radar, heatmap) so pre-launch gaps cannot skew stats.

### `41d104a` — feat: add Logbook UI and renderLogbook()
- Add a Logbook card listing all days with non-empty notes, newest first, showing date, done/missed counts, the sanitized note, and done/missed habit lists with Lucide icons.
- Hook `renderLogbook()` into `renderAllViews()` so the journal refreshes with the rest of the UI.

### `380b941` — feat: promote index7 to index.html; add scroll parallax; update docs
- Promote the Firebase/Chart.js `index7.html` to be the production `index.html`; archive the legacy Mavo/Sheets page verbatim as `oldindex.html`.
- Add `initScrollParallax()`: decorative Lottie lanes (`#laneLeft`, `#laneRight`) translate horizontally with scroll using rAF-throttled passive listeners.
- Rewrite `AGENTS.md` for the Firebase app (auth/demo mode, compact `habitsY` storage, SEO, UX guidelines); reformat markup Prettier-style.

## 2025-11-05

### `f20d700` — feat: add SEO meta, Lottie drifters, tip carousel, checkbox pop
- Add SEO head tags (canonical to habits.roomtolearn.org, description, robots, Open Graph, Twitter card, theme-color, SVG favicon) plus JSON-LD `WebSite` structured data.
- Add fixed decorative side lanes with dotlottie-wc "drifter" animations that fall via CSS keyframes, hidden below 728px.
- Replace the single daily tip with a 29-tip carousel (copy from `weekly-habit-tracker-notes.md`) cycling every 4s with a fade; add a checkbox "pop" animation on habit toggle; remove the card hover-lift effect.

### `42396dd` — feat: revamp index7 UI/analytics and add index8 dark dashboard
- Rebuild `index7.html`: unified card/button/input styles, rebuilt stats overview and 3-day tracker, notification queue, debounced autosave, and four new Chart.js analytics (weekly bar, habit radar, 30-day trend, habit matrix) with live stat updates.
- Improve data handling: cleaner date utilities, localStorage/Firebase read-write paths, encouragement notifications on habit toggles.
- Add `index8.html`: dark modular dashboard on Firebase v10 modular SDK with Google sign-in, rolling 3-day check-ins, public week/month views, and a dense bitmask storage format.
- Reorganize and expand `weekly-habit-tracker-notes.md` (circadian context, weekly rhythm, fasting, light strategy, wind-down ritual, CBT/habit design, seasonal anchors).

### `ea9d0de` — feat: unify styling and refactor UI components
- Introduce shared CSS component classes (`.btn`, `.card`, `.input`, `.link`, `.label-chip`, hover shadows) and replace scattered Tailwind utilities/inline styles with them.
- Refactor the auth modal, header, tip card, recent-habits section, and analytics cards to the new classes; standardize icon sizing and simplify habit item markup; tighten daily tips copy.

### `48fa96f` — feat: add multiple habit-tracker UIs, Mavo docs and todo UI polish
- Add `index4.html`–`index7.html`: parallel single-file variants exploring Mavo 3-day rolling views, weekly/monthly dashboards, GitHub storage, and a Firebase-backed weekly tracker with auth, charts, notifications, and analytics.
- Update `index2.html` to customize/hide the default Mavo toolbar and use Mavo's built-in save flow; update `goal.md` for GitHub JSON storage.
- Overhaul `todo.html`: drag-handle reordering, j/k/x/e/Delete keyboard shortcuts, edit-in-place, clearer stats, more robust persistence.
- Add Mavo reference docs: `mavo-api.md`, `mavo-expressions.md`, `style-mavo.md`.

### `1b1e958` — chore(data): record no-op Mavo save of habitTracker.json
- Mavo's GitHub storage backend committed a save whose tree is identical to its parent; kept as history noise from the automation.

### `7da400b` — chore(data): clean whitespace placeholders from habitTracker.json
- Mavo auto-save drops whitespace-only `today.*` habit entries, trims newline padding from time values, and empties the `recentDays` array.

### `8e9cf0d` — chore(data): persist initial Mavo form placeholders to habitTracker.json
- First persisted Mavo form state: default empty `today.*` habit fields, a template `weekHabits` row, and an empty `recentDays` array written to GitHub storage.

### `60ab66c` — chore(data): add initial habitTracker5.json Mavo data
- Seed `habitTracker5.json` with the first Mavo save of habit fields (exercise, sunlight, breakfast, protein, sleep, winddown, noscreens) and empty `weekHabits`/`recentDays` placeholders.

### `e87ff22` — feat: switch Mavo storage/auth to GitHub and add seed habits.json
- Replace Google Sheets storage/auth in `index2.html` with Mavo's GitHub backend (`mv-storage` → repo URL, `mv-auth="github"`).
- Rename login/logout handlers and UI text from Google to GitHub; drop a Google-specific OAuth redirect workaround.
- Add `habits.json` seed file with two example daily habit entries.

### `28f4fd7` — chore(data): no-op save of habitTracker.json
- Mavo auto-save produced no changes; tree identical to parent commit, recorded to keep GitHub storage history in sync.

### `e422545` — chore(data): add initial habitTracker.json Mavo data
- Seed the Mavo storage file with zeroed week/month computed stats (exercise/sleep/nutrition counts, score, streaks) and a placeholder `habitData` entry from the first app save.

### `678c65a` — fix: improve Mavo Google auth flow, login/logout and app initialization
- Switch auth to the Mavo storage API (`mavoApp.storage.login/logout`) instead of the broken `mavoApp.login` path; remove a manual OAuth2 fallback with placeholder client ID and token-in-hash handling.
- Wait on `Mavo.ready` with a 1s retry; read auth status from `mavoApp.storage.user` and listen for `mv-login`/`mv-logout` events; tolerate OAuth redirect errors.
- Point `mv-storage` at the spreadsheet with `mv-storage-sheet="Week2"`; tidy auth button markup and EDIT/VIEW mode toggling.

## 2025-11-04

### `9202846` — feat: add AGENTS.md, daily/dashboard UIs and starter CSVs; refine auth UI & add Mavo debug logs
- Add `AGENTS.md` repo guidelines, two new Mavo UI prototypes (`index2.html` daily-entry tracker, `index3.html` daily + dashboard variant with the gsheets plugin), and `init2.csv`/`init3.csv` seed data.
- Rewrite `goal.md` to target Google Sheets and spell out tracking fields plus signed-in/signed-out UX.
- Refine `index.html` auth UI: hide overlay/help when viewing, auth pill only when logged in, Save visible only in edit mode; add Mavo debug logging and `mv-edit`/`mv-save` event wiring.

### `931fda2` — feat(sheets): switch tracker to Google Sheets; add auth UI & CSV export
- Switch `index.html` from the Mavo GitHub storage plugin to gsheets (`mv-plugins="gsheets"`, `mv-storage` URL, `mv-storage-sheet="Week"`, `transformHeadings`).
- Add an auth UI layer (signed-in pill, login/logout, auth-help, sign-in overlay), an Open Sheet link, a client-side 7-day CSV template button, and a Reset Week action.
- Add reference docs `expressions.md` (Mavo expressions) and `sheets.md` (gsheets plugin).

### `768034f` — feat: add initial Mavo-based weekly habit tracker UI and goal.md
- Bootstrap the repo with `index.html`: a single-page Mavo app using GitHub storage, Tailwind CDN, Inter/Space Mono typography, Lucide icons, a custom edit/save toolbar, weekly overview cards, and a 7-day habit checklist grid (morning light, workout, walk, fasting, guitar, stretch, journal, social night).
- `goal.md` records the design prompt and requirements.

### `b1432db` — Add comprehensive documentation and UI prototypes
- Add `final-mavo.md` (full Mavo.io guide and example index) and `style.md` (canonical visual system: colors, 2px borders, offset shadows, typography, Tailwind mappings).
- Add `weekly-habit-tracker-notes.md`, the circadian/fasting source content, and `todo.html`, a styled interactive Todo prototype with localStorage persistence, drag-to-reorder, filters, and stats.
