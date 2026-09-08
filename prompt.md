# One-Shot Recreation Prompt — Habit Tracker (Weekly Rhythm)

Give this prompt to a capable coding agent to recreate this application from
scratch in a single session. It captures every decision the original embodies.

---

## Prompt

Build a single-file HTML habit-tracker web app called **Habit Tracker —
Weekly Rhythm**. Everything — markup, CSS, and JavaScript — lives in one
`index.html` with no build step; all libraries load from CDNs. The app tracks
a 12-habit circadian daily rhythm, stores data in Firebase Realtime Database
under a compact yearly bitset, shows public read-only analytics when signed
out, and full editing when signed in with Google.

### Stack (CDN only, exact)

- Tailwind CSS via `https://cdn.tailwindcss.com` (utility classes in body
  markup; custom tokens via CSS variables in an inline `<style>` block).
- Lucide icons via `https://unpkg.com/lucide@latest`, 1.5 stroke width;
  re-run `lucide.createIcons()` after every dynamic DOM injection.
- Chart.js via `https://cdn.jsdelivr.net/npm/chart.js` for all charts.
- Firebase v9 **compat** SDK scripts: `firebase-app-compat.js`,
  `firebase-auth-compat.js`, `firebase-database-compat.js` (9.22.0).
- dotlottie-wc web component (`@lottiefiles/dotlottie-wc@0.8.5`) for
  decorative animations.
- Google Fonts: Inter (300/400/500) for body; Space Mono (400) for
  labels/chips.
- Embed a `firebaseConfig` object with the standard web app fields
  (apiKey, authDomain, databaseURL, projectId, storageBucket,
  messagingSenderId, appId) — public client config, no secrets.

### Design language (strict; "2px paper-card" neobrutalism)

- Ink `#383838` on paper `#F4EFEA`; white `#FFFFFF` cards; yellow
  `#FFDE00` highlights; blue `#6FC2FF` primary buttons; focus blue
  `#2BA5FF`; muted `#A1A1A1`. Accent palette for tiles: salmon `#FF7169`,
  teal `#53DBC9`, purple `#B291DE`, lime `#B3C419`, gold `#E1C427`, steel
  `#84A6BC`, periwinkle `#7597EE`.
- Every card and control: 2px solid ink border, 2px radius, hard offset
  shadow `box-shadow: -8px 8px 0 0 #383838`. **No gradients, no soft
  shadows, no bold font weights.**
- Uppercase titles and chips (Space Mono or Inter 400 uppercase) instead of
  bold; tracking-tight on titles >20px; generous whitespace; 8-pt spacing
  scale.
- Buttons lift toward their shadow on hover (`translate(7px, -7px)`), reset
  on active.
- Checkbox "pop" animation on toggle: scale bounce restarted via forced
  reflow; each checkbox gets a unique id.
- Animations are CSS-only. Decorative side "drifter" Lottie animations in
  fixed left/right lanes outside the content column: fall downward via CSS
  keyframes, shift horizontally with scroll (rAF-throttled, passive
  listener, `#laneLeft` / `#laneRight`), hidden below 728px viewport width.
- Responsive: content column centered, max ~960px; checks at <728px,
  ~960px, ≥1302px. Visible focus outlines; keyboard-operable controls.

### Data model

- `habitsConfig`: array of 12 habits, append-only (index = bit position;
  never reorder). Each: `{ id, name, time, icon, category }` where category
  is `morning | work | evening`:
  - morning: `morningLight` (Morning Light, 5-7 AM, sun),
    `exercise` (Exercise, 6-7 AM, dumbbell), `breakfast` (Breakfast, 7-8
    AM, coffee)
  - work: `deepWork` (Deep Work, 8 AM-12 PM, brain), `lunch` (Lunch, 12
    PM, utensils), `afternoonWalk` (Afternoon Walk, 3 PM, footprints)
  - evening: `startFast` (Start Fast, 5 PM, clock), `creativeWork`
    (Creative Time, 5-7 PM, palette), `guitar` (Guitar, 8-8:30 PM, music),
    `stretching` (Stretching, 8:30-8:45 PM, heart), `voiceJournal` (Voice
    Journal, 8:45-9 PM, mic), `bedtime` (Bedtime, 9 PM, moon)
- In-memory `habitsData`: `{ ["YY-DDD"]: { habits: {id: boolean}, notes:
  string, completed: number, total: number } }` where the key is 2-digit
  year + day-of-year (e.g. `25-308`).
- Database paths (RTDB):
  - Compact (preferred): `habitsY/<uid>/<year>` = `{ h, n, v }` —
    `h`: Base64 bitset, bit index `(day-1) * H + habitIndex` (H = 12;
    ~0.6 KB/year); `n`: sparse `{ [dayOfYear]: note }`; `v`: 1.
  - Legacy (compat): `habitsTracker/<uid>` storing the verbose per-day
    object.
- Load: prefer `habitsY`, fall back to `habitsTracker`. Save (signed in
  only, debounced autosave): write **both** paths.
- `LOGBOOK_MIN_DATE = new Date(2025, 10, 4)` (Nov 4, 2025) is the data
  epoch: no statistic, chart, or navigable page may include earlier days.

### Auth and demo mode

- Google OAuth via Firebase Auth (`signInWithPopup`). Header: "Sign in
  with Google" when signed out; user email + "Sign Out" when signed in.
- Signed out: load demo account `DEFAULT_DEMO_UID =
  "HkjWermUkCdEMbTJxSswSBSGCap2"`, render all analytics read-only,
  disable every editing control; charts stay visible.
- Signed in: full editing against the user's UID.
- (Hosting side: RTDB rules allow public read; writes only where
  `auth.uid == $uid`; writes to the demo UID blocked.)

### Functions to implement (by name)

- Dates: `getDayKey(date)` → `"YY-DDD"`, `getDateString(date)`,
  `getDateFromKey(key)`, `isLeapYear(y)`, `daysInYear(y)`.
- Codec: `getHabitIndexMap()`, `encodeYearCompact(data, year)` →
  `{h, n, v:1}`, `decodeYearCompact(compact, year)` → `habitsData` (with
  `b64FromBytes` / `bytesFromB64` helpers).
- Data: `loadHabits()`, `saveHabits()`, `autoSave()` (debounced).
- Auth: `initAuth()`, `signInWithGoogle()`, `signOut()`, `updateAuthUI()`.
- Views: `renderAllViews()` → `updateStats()`, `renderCharts()`
  (`renderWeeklyChart`, `renderHabitRadarChart`, `renderMonthlyTrendChart`,
  `renderHabitMatrixChart`), `renderRecentHabits()`, `renderLogbook()`,
  `startTipsCarousel()`, `initDrifters()`, `initScrollParallax()`.
- Interaction: `toggleHabit(dayKey, habitId)`, `updateNotes(dayKey, value)`,
  `changeRecentPage(deltaDays)` (3-day pages), `changeLogbookWeek(delta)`
  (Sun–Sat pages).
- Notifications: `queueNotification(message, icon, color)` +
  `processNotificationQueue()` — serialized encouraging toasts on habit
  toggles.
- Init on `DOMContentLoaded`.

### UI sections (top to bottom)

1. **SEO head**: canonical `https://habits.roomtolearn.org/`, description,
   robots, Open Graph (url/title/description/image/image:alt), Twitter
   `summary_large_image`, `theme-color` `#F4EFEA`, SVG favicon (yellow sun
   on ink strokes), JSON-LD `WebSite` schema.
2. **Header**: logo (letters, tight tracking), auth pill/sign-in button.
3. **Stats overview** tiles: Today score %, Week streak, Monthly average %,
   Total days (since `LOGBOOK_MIN_DATE`, inclusive) — 2px cards with label
   chips, values update live.
4. **Tip carousel**: single tip card cycling every 4s with a fade through
   29 short encouragement lines (theme: ending the day well, closure over
   stimulation, letting momentum settle — e.g. "End the day with
   completion, not continuation.", "Rest amplifies tomorrow.", "Closure >
   Stimulation.", "Landing beats crashing.").
5. **Recent Progress tracker**: 3-day page of day-cards (date label; the
   12 habit checkboxes grouped by category with icons and time windows; a
   notes textarea per day); prev/next pagination with a range label,
   clamped so the oldest day ≥ `LOGBOOK_MIN_DATE`; buttons disable/grey
   out at bounds.
6. **Analytics grid**: weekly bar chart (per-day completion), habit radar
   (per-habit completion %), 30-day trend line, month habit matrix
   (heatmap). Wrap each canvas in a `<div>` (bare canvas siblings of text
   nodes trigger a Chart.js infinite-resize bug).
7. **Logbook**: journal of all days having notes, newest first — date,
   done/missed counts, HTML-escaped note (pre-wrap), done/missed habit
   lists with Lucide icons; one Sun–Sat week per page, prev/next
   pagination clamped to `LOGBOOK_MIN_DATE`; empty days hidden; re-renders
   on note edits.
8. **Decorative Lottie lanes** flanking the content (see design language).

### Build order

1. HTML skeleton + design tokens + fonts/CDN includes.
2. `habitsConfig`, date utilities, in-memory `habitsData`, localStorage
   persistence.
3. Auth (Firebase) + demo mode + `updateAuthUI`.
4. Storage codec (`encodeYearCompact`/`decodeYearCompact`) + RTDB
   load/save (both paths) + debounced autosave.
5. Recent tracker (checkboxes, notes, pop animation, notifications).
6. Stats + four charts (respect `LOGBOOK_MIN_DATE` everywhere).
7. Logbook + pagination for both Recent and Logbook.
8. Tips carousel, drifters, scroll parallax, SEO head/JSON-LD/favicon.

### Acceptance criteria

- Opening `index.html` from a static server (e.g. `python3 -m
  http.server`) with no other setup renders the full dashboard.
- Signed out: demo analytics render; every editing control is disabled;
  no saves occur.
- Signed in: toggling a habit pops the checkbox, queues an encouraging
  notification, updates Today score/streaks/charts immediately, and
  autosaves (debounced) to both `habitsY` and `habitsTracker` paths.
- Reload after edits: state persists via `habitsY` (bitset round-trips
  exactly, including notes).
- One year of 12-habit data serializes to well under 10 KB in `h` (plus
  sparse notes).
- Week streak stops at `LOGBOOK_MIN_DATE`; monthly average, total days,
  and all four charts exclude earlier dates.
- Recent pagination cannot page past `LOGBOOK_MIN_DATE`; Logbook pages in
  Sun–Sat weeks, hides empty days, and escapes note HTML.
- No `habitsConfig` reordering ever occurs (bit positions depend on
  indices); new habits are appended.
- Visual language holds everywhere: 2px ink borders, 2px radius, offset
  shadows, no gradients, uppercase chips, Lucide 1.5 icons.
- Below 728px the Lottie lanes disappear; layout remains usable at
  ~960px and ≥1302px.
