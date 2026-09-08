# 003 — Compact yearly bitset data model (`habitsY` `{h, n, v}`)

- **Date:** 2025-11-05 (prototyped in `index8.html`, productionized in `index7`→`index.html`)
- **Commits:** `42396dd`, `380b941`
- **Status:** accepted

## Context

The original requirement (`goal.md`): one year of habit data must fit in
10 MB, UTF-8, keyed compactly (`25-308` = day 308 of 2025), computed
client-side. A verbose per-day JSON object easily blows past comfortable
sizes and makes cloud reads slow.

## Decision

Store each year at `habitsY/<uid>/<year>` as:

- `h`: Base64-encoded bitset of `daysInYear × habitsCount` bits, row-major
  (day-major: bit index = `(day-1) * H + habitIndex`). Bit set ⇔ habit done.
- `n`: sparse notes map `{ [dayOfYear]: string }` — only days with notes.
- `v`: schema version (starts at 1).

`encodeYearCompact` / `decodeYearCompact` translate between this and the
in-memory `habitsData` shape (`{ [YY-DDD]: { habits: {id:true}, notes,
completed, total } }`). Loading prefers `habitsY` and falls back to legacy
`habitsTracker/<uid>`; saving writes both.

## Consequences

- 12 habits × 365 days ≈ 4,380 bits ≈ 548 bytes → ~0.6 KB Base64 per year
  vs tens of KB verbose. Notes now dominate size; they are kept short by
  convention.
- **Critical invariant:** habit bit positions are indices into
  `habitsConfig`. Reordering the array corrupts every stored year — new
  habits may only be appended.
- Changing habit count mid-year leaves stale bits for habits beyond the
  current count; they are ignored by the decoder (H is read from the current
  config).
- Dual-writing keeps old readers working but doubles writes; retirement of
  the legacy path is a pending decision.
