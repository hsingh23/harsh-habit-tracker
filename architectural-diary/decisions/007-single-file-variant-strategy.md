# 007 — Parallel single-file variants; winner takes `index.html`

- **Date:** 2025-11-04 → 2025-11-06
- **Commits:** `9202846`, `48fa96f` (variants created), `42396dd` (index8), `380b941` (promotion)
- **Status:** accepted

## Context

With no build system, no tests, and a single-author workflow, the fastest way
to evaluate design/storage/layout ideas was to duplicate the whole app into a
new HTML file and diverge.

## Decision

Develop as parallel, self-contained single-file apps: `index2`–`index8`, each
a complete tracker exploring one axis (Mavo daily entry, dashboards, 3-day
rolling, GitHub storage, Firebase compat, Firebase v10 modular + bitmask).
When `index7.html` (Firebase + Chart.js + compact storage) proved best, it
was promoted to `index.html` in `380b941`; the previous incumbent was
preserved verbatim as `oldindex.html`; the winner's old name was deleted.
Superseded variants were never deleted — they remain as archaeology.

## Consequences

- Pros: zero-risk experimentation; each variant runs by simply opening it;
  promotion is a rename; rollback is keeping the old file.
- Cons: heavy duplication (six ~25–75 KB files share conceptual logic that
  drifted apart); "which file is real?" ambiguity — resolved only by the
  promotion commit and by documentation (README, AGENTS.md).
- Rules that emerged: only `index.html` is production; variants are
  read-only reference; `index7`'s name lives on inside `index8.html`'s title
  ("Habit Tracker · Index7") as a naming artifact of the era.
- Future direction (not yet decided): extract shared data-layer logic if a
  variant strategy is ever needed again, or accept the archaeology and move
  on.
