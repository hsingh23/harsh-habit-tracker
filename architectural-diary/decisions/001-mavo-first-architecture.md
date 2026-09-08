# 001 — Start declarative: Mavo as the first architecture

- **Date:** 2025-11-04
- **Commits:** `768034f` (initial Mavo UI), `b1432db` (reference docs), `678c65a` (auth fix)
- **Status:** superseded (see 002, 007)

## Context

The project began with rich reference material (`final-mavo.md`, `style.md`,
`weekly-habit-tracker-notes.md`) and a goal of shipping a habit tracker fast.
Mavo — a declarative HTML extension with storage, auth, and expressions via
`mv-*` attributes — promised an app with zero custom JavaScript and zero
backend code.

## Decision

Build the first `index.html` as a Mavo app: `mv-app` root, `mv-storage`
pointing at GitHub, habit toggles driven by `mv-action` expressions, weekly
stats computed in MavoScript, and a custom toolbar replacing Mavo's default.

## Consequences

- Pros: a working weekly tracker in ~280 lines; data model emerges directly
  from markup; auto-save to GitHub "just worked" (too well — see the
  `chore(data)` no-op commits in the CHANGELOG).
- Cons: auth edge cases fought the framework (fixed only by dropping into
  `mavoApp.storage.login` and waiting on `Mavo.ready` with retries in
  `678c65a`); charting and rich interactivity were out of reach; storage
  backends were plugin-dependent.
- The declarative experiment established the data vocabulary (per-day habit
  booleans, times, notes) that survived into the Firebase rewrite.
