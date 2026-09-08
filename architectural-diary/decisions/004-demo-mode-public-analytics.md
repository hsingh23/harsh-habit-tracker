# 004 — Public-read demo mode with a fixed demo UID

- **Date:** 2025-11-05
- **Commits:** `42396dd` (first Firebase auth UX), `380b941` (documented in AGENTS.md)
- **Status:** accepted

## Context

`goal.md` required: "data is read publicly, but have to login to edit." The
app should never show an empty shell to anonymous visitors — the dashboards
and analytics are the product's public face.

## Decision

Database rules allow public reads. When no user is signed in, the app loads
data for `DEFAULT_DEMO_UID` (`HkjWermUkCdEMbTJxSswSBSGCap2`) and renders all
charts/stats read-only: checkboxes and inputs disabled, no save path, header
shows "Sign in with Google". Signed in, the app reads/writes the user's own
UID with full editing.

## Consequences

- Anonymous visitors get a rich, real-data first impression instead of an
  auth wall.
- The demo account's data must stay presentable (it is public).
- Security rules must block writes to the demo UID (otherwise vandals could
  deface the public landing view); rules also enforce `auth.uid == $uid`
  elsewhere.
- Auth state is the single switch for edit capability everywhere in the UI
  (`updateAuthUI`), which keeps the read-only/edit split consistent.
