# Habit Tracker — Weekly Rhythm

A single-page, single-file habit tracker for running a circadian-aligned daily
rhythm: check off habits, take per-day notes, and watch streaks and trends on
a playful, paper-card dashboard. Data is public-read (anyone can view the
dashboards and analytics) and edit-gated behind Google sign-in, stored in
Firebase Realtime Database in a compact yearly bitset format.

Live at `https://habits.roomtolearn.org/`.

## Why

The tracker implements a personal "weekly rhythm" system (documented in
`weekly-habit-tracker-notes.md`): fixed wake/sleep anchors, morning light,
fasting windows, and a night wind-down ritual. The app exists to make that
system checkable in seconds each day, to surface encouragement (tips carousel,
encouragement notifications on toggles), and to prove consistency with
streaks, radar, trends, and a month heatmap — without a backend server.

## Features

- **12-habit daily checklist** grouped by morning / work / evening categories,
  driven by a single `habitsConfig` array (id, name, time window, Lucide icon).
- **Recent Progress tracker**: rolling 3-day pages with prev/next pagination
  and per-day notes.
- **Logbook**: journal-style history of every day with notes — done/missed
  counts, sanitized note text, habit lists — paged one Sun–Sat week at a time.
- **Analytics** (Chart.js): weekly bar chart, habit radar, 30-day trend, month
  habit matrix/heatmap, today score, week streak, monthly average, total days.
- **Stats anchored to a start date**: `LOGBOOK_MIN_DATE` (Nov 4, 2025) — days
  before it are excluded from streaks, averages, totals, and charts.
- **Auth-gated editing**: Google sign-in (Firebase Auth). Signed out, the app
  renders read-only analytics from a demo account; controls are disabled.
- **Encouragement layer**: 29-tip carousel cycling every 4s (copy from
  `weekly-habit-tracker-notes.md`) and queued notifications on habit toggles.
- **Compact storage**: one year of habit checks stored as a Base64 bitset
  (~0.6 KB for 12 habits) plus a sparse notes map — far under the 10 MB cap.
- **Polish**: 2px-border paper-card design language (see `style.md`), checkbox
  pop animation, decorative Lottie "drifters" with scroll parallax (hidden on
  small screens), full SEO/OG/Twitter/JSON-LD head.

## Stack

- **No build step.** One HTML file (`index.html`) loaded directly in a browser.
- Tailwind CSS (CDN), custom CSS variables for the design tokens.
- Chart.js (CDN) for all charts; Lucide (CDN) for icons (1.5 stroke width).
- Firebase v9 compat SDK (CDN): Auth (Google OAuth) + Realtime Database.
- dotlottie-wc web components for decorative animations.
- Fonts: Inter (300/400/500) for body, Space Mono for labels/chips.

## Quickstart

```bash
# from the repo root — any static file server works
python3 -m http.server 8000
# open http://localhost:8000/index.html
```

There is nothing to install or build; all dependencies load from CDNs.

To edit data you must sign in with Google against the configured Firebase
project; without sign-in the app shows read-only demo analytics.

## Repository structure

```
index.html                    # The app: Firebase habit tracker (production)
index2.html ... index5.html   # Mavo-based prototype variants (daily/dashboards)
index6.html, index8.html      # Firebase prototype variants (index8: dark, modular SDK)
oldindex.html                 # Legacy Mavo/Google Sheets page, archived verbatim
todo.html                     # Standalone todo-app prototype (design reference)
habits.json, habitTracker*.json, init*.csv  # Seed/sample data files
style.md                      # Canonical visual language (colors, type, spacing)
weekly-habit-tracker-notes.md # Source content: circadian system + tips copy
final-mavo.md, mavo-api.md, mavo-expressions.md, style-mavo.md,
expressions.md, sheets.md     # Mavo/Sheets reference docs (legacy era)
goal.md                       # Original build prompt/requirements
AGENTS.md                     # Agent + contributor guidelines
CHANGELOG.md                  # Every commit, newest first
architectural-diary/          # Design decisions and history
prompt.md                     # One-shot recreation prompt for this app
```

## Configuration

The Firebase web config (apiKey / authDomain / databaseURL / projectId /
storageBucket / messagingSenderId / appId) is embedded in `index.html` as
`firebaseConfig`. These are public client identifiers for the web app; write
access is controlled by Firebase security rules, which should restrict writes
to `auth.uid == $uid` and block writes for the demo UID. No secret keys belong
in this repository.

## Notes

- The compact storage path is `habitsY/<uid>/<year>` (shape `{ h, n, v }`);
  the legacy verbose path `habitsTracker/<uid>` is still written for
  compatibility and read as a fallback. See `AGENTS.md` for the full data
  model and its invariants (e.g., never reorder `habitsConfig`).
- `index2`–`index8` are historical experiments kept for reference; only
  `index.html` is the production app.
