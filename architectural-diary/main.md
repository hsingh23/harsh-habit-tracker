# Architectural Diary — Habit Tracker (Weekly Rhythm)

A narrative history of how this repository's architecture evolved, reconstructed
from the git history (post-rewrite hashes; see `CHANGELOG.md` for the per-commit
view and the history-rewrite note).

## Timeline

### Phase 0 — Reference material (Nov 4, 2025 · `b1432db`)
The repo opens not with code but with a design kit: `style.md` (the 2px
paper-card visual language), `weekly-habit-tracker-notes.md` (the circadian
system the app will track), `final-mavo.md` (a full Mavo.io guide), and
`todo.html`, a working todo prototype whose interactions seed the eventual
habit-checking UX.

### Phase 1 — Mavo first cut (`768034f`)
`index.html` debuts as a declarative Mavo app with GitHub storage: a weekly
habit checklist driven entirely by `mv-*` attributes and expressions, plus
`goal.md` recording the requirements. No JavaScript backend of our own.

### Phase 2 — Storage backend churn (`931fda2` → `e87ff22`)
The storage story flails productively: Google Sheets via the Mavo gsheets
plugin (with auth UI, CSV template, Reset Week), back to GitHub JSON storage,
with a detour fixing the Mavo Google auth flow (`678c65a` — wrong login API,
OAuth redirect handling). Mavo's GitHub backend auto-commits data saves, which
is why history contains several `chore(data)` no-op/seed commits. Lesson
recorded: storage automation writes commit noise; declarative backends fight
you on auth edge cases.

### Phase 3 — Variant explosion (`9202846`, `48fa96f`)
Six parallel single-file variants (`index2`–`index7`) explore the design
space: Mavo daily entry, daily+dashboard, 3-day rolling, GitHub storage, and
finally `index7.html` — Firebase v9 compat + Chart.js + auth + notifications.
`todo.html` gains keyboard shortcuts and drag reordering. The repo becomes a
laboratory; each variant is a complete app.

### Phase 4 — The Firebase app matures (`ea9d0de`, `42396dd`, `f20d700`)
`index7` gets shared CSS component classes, then a full revamp: unified
styles, notification queue, debounced autosave, and four Chart.js analytics.
`index8.html` appears as a dark, Firebase-v10-modular dashboard prototyping a
dense bitmask format. SEO/OG/JSON-LD head, Lottie drifters, tip carousel, and
checkbox pop land, preparing the app for public hosting at
habits.roomtolearn.org.

### Phase 5 — Promotion and closure (`380b941`, `41d104a`, `0dbefb9`)
`index7.html` is promoted to `index.html`; the legacy Mavo page is archived as
`oldindex.html`; `AGENTS.md` is rewritten for the Firebase architecture. The
Logbook (journal of note-days) is added, then both the Recent tracker and
Logbook gain pagination clamped to `LOGBOOK_MIN_DATE` — Nov 4, 2025, the date
real data began — with all analytics excluding earlier dates so pre-launch
gaps cannot poison streaks and averages.

### Phase 6 — Documentation (Sep 8, 2026)
Commit messages improved via a messages-only history rewrite (content
preserved; see CHANGELOG rewrite note). This diary, `README.md`, `AGENTS.md`,
`CHANGELOG.md`, and `prompt.md` (one-shot recreation) written.

## Decision records

| # | Decision | File |
|---|----------|------|
| 001 | Start declarative: Mavo as the first architecture | `decisions/001-mavo-first-architecture.md` |
| 002 | Storage backend migrations: GitHub → Sheets → Firebase RTDB | `decisions/002-storage-backend-migration.md` |
| 003 | Compact yearly bitset data model (`habitsY` `{h,n,v}`) | `decisions/003-compact-bitset-data-model.md` |
| 004 | Public-read demo mode with a fixed demo UID | `decisions/004-demo-mode-public-analytics.md` |
| 005 | 2px paper-card neobrutalist design language | `decisions/005-neobrutalist-design-language.md` |
| 006 | `LOGBOOK_MIN_DATE` stat anchoring + pagination | `decisions/006-logbook-min-date-stat-semantics.md` |
| 007 | Parallel single-file variants, winner-takes-index.html | `decisions/007-single-file-variant-strategy.md` |

## Ongoing tensions

- Single-file architecture keeps deployment trivial but `index.html` is
  ~2200 lines; the variant files are the pressure valve.
- Dual-write storage (`habitsY` + legacy `habitsTracker`) preserves
  compatibility at the cost of write amplification and migration risk.
- No test suite: correctness rests on the manual checklist in `AGENTS.md`.
