# 006 — `LOGBOOK_MIN_DATE` stat anchoring and history pagination

- **Date:** 2025-11-06
- **Commits:** `41d104a` (Logbook), `0dbefb9` (pagination + min-date exclusions)
- **Status:** accepted

## Context

Real data begins Nov 4, 2025 (launch day). Before pagination, the Recent
tracker showed a fixed 3-day window and there was no way to browse history;
naive streak/average math over an unbounded past would count the empty
pre-launch days as failures, tanking every statistic.

## Decision

1. Introduce a single constant `LOGBOOK_MIN_DATE = new Date(2025, 10, 4)`
   as the epoch for all analytics:
   - week streak loop breaks when it walks past the min date;
   - monthly average and `totalDays` only count days ≥ min date;
   - weekly chart, radar, 30-day trend, and habit matrix exclude/zero
     earlier dates.
2. Add bounded history browsing instead of unbounded lists:
   - Recent Progress pages through 3-day windows (`recentOffsetDays`);
   - Logbook pages through Sun–Sat weeks (`logbookWeekOffset`);
   - both clamp so the oldest visible day never precedes `LOGBOOK_MIN_DATE`;
   - nav buttons disable/grey out at the bounds.

## Consequences

- Streaks and averages are honest from day one (no phantom missed days).
- One constant now defines "when data exists"; moving the launch date later
  means editing one line and every chart/stat follows.
- Pagination state is UI-only (not persisted), so reloads return to today —
  a simplicity-over-convenience trade that has held.
- Logbook rendering hides empty days and HTML-escapes notes (a mini XSS
  guard for user-authored text rendered as HTML).
