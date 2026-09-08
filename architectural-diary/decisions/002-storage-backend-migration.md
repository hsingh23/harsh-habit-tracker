# 002 — Storage backend migrations: GitHub → Google Sheets → Firebase RTDB

- **Date:** 2025-11-04 → 2025-11-05
- **Commits:** `931fda2` (Sheets), `e87ff22` (GitHub JSON), `48fa96f` (Firebase variants), `380b941` (promotion)
- **Status:** accepted (Firebase Realtime Database is final)

## Context

The tracker needed: public read access, gated writes, ~10 MB/year of data,
and no server. Three backends were tried in rapid succession.

## Decisions, in order

1. **Mavo + GitHub storage** (`768034f`, `e87ff22`): data committed as JSON
   to the repo. Worked, but every save created a commit (several no-op ones
   pollute history), and OAuth was fiddly.
2. **Mavo gsheets plugin** (`931fda2`, `9202846`): `mv-storage` → Google
   Spreadsheet with `mv-storage-sheet`, CSV template seeding (`init2.csv`,
   `init3.csv`). Nice for hand-editing data; poor for programmatic reads and
   public dashboards.
3. **Firebase Realtime Database** (`48fa96f` → `380b941`): custom JS, Google
   OAuth via Firebase Auth, RTDB paths `habitsTracker/<uid>` (verbose) and
   `habitsY/<uid>/<year>` (compact). This is the production choice.

## Consequences

- Firebase gives public-read rules with per-UID write gating and real-time
  cloud storage with no servers — the exact requirements.
- Compatibility burden: the verbose `habitsTracker` path must still be
  written and read as fallback (dual-write in `saveHabits`).
- The abandoned backends remain as `oldindex.html`, `index2`–`index5` and
  the `sheets.md`/`expressions.md` docs — kept as reference, never deleted.
