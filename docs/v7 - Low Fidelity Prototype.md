# Version 7: Low-Fidelity Prototype

## Blocking Questions — Resolved

### 1. Day-Tab Pattern
**Decision:** Universal day-tabs (`[THU][FRI][SAT][SUN]`), applied on both mobile and desktop, replacing V5's desktop flat table.

**Defense:** Chosen primarily for build simplicity as a solo developer — one component, one interaction model, instead of maintaining two divergent layouts per breakpoint. This overrides V5's desktop wireframe, so the tradeoff is stated explicitly here rather than inherited by accident.

**Mitigation for Pecco (desktop, glance-while-working persona):** The tab bar **auto-jumps to the current day** whenever a race weekend is active, so his primary use case (checking the next session) costs zero clicks — identical friction to the flat table it replaces.

**Off-week default:** No "today" exists to jump to, so the tab bar defaults to **THU**, matching the first day of on-track action for the upcoming GP. Deterministic, no ambiguity.

### 2. Column Ordering
**Decision:** **LOCAL TIME** (left) → **REMOTE TIME** (right), on both breakpoints.

**Defense:** Pecco's primary question is "when does this start, in my timezone?" Left-to-right reading order puts the answer to that question first. Remote/track time is secondary context.

### 3. Contenders Block Layout
**Decision:** Cards, not a mini-table. Wrapping grid, max 4 per row on desktop; single column on mobile.

**Status:** Carried forward unchanged from V5 — not reopened in V7.

### 4. Live Session State
**Decision:** When a session is active, the countdown timer is replaced by a high-visibility "LIVE NOW" badge, and the active session's card in the schedule list receives a distinct highlight.

**Defense:** Removes ambiguity about whether "the countdown hit zero" means live-now or stale — the state change is visually unmistakable.

### 5. Navigation Architecture (Regression Rollback)
**Decision:** Reaffirmed strict 3-tab IA from V4 — Schedule/Home, Standings, Riders. No fourth "Dashboard" destination, no merging Standings into the Home view on desktop.

**Defense:** Desktop real estate does not justify breaking the locked mental model. On wider viewports, the Schedule view may use the extra horizontal space for an expanded session table and the 4-per-row contenders grid — but it does not pull in the full standings table. This corrects a regression that appeared in an early desktop mockup.

## Screen-State Inventory

| State | Trigger | Behavior |
|---|---|---|
| **Default** | Upcoming session exists | Countdown timer, day-tab auto-jumped to today |
| **Live** | Active session in progress | Countdown → "LIVE NOW" badge; active card highlighted |
| **Off-week** | No race this weekend | Day-tab defaults to THU; schedule shows provisional timings for next GP |
| **Off-week, no provisional data** | Backend hasn't published next-GP timings yet | Empty state: "Schedule TBA" (locked in V5) |
| **Loading** | Initial data fetch in progress | Skeleton/shimmer placeholders, no layout shift |
| **Error — Schedule** | Fetch fails, cache exists | Render cached schedule; passive non-blocking banner: "⚠️ Offline – Showing cached schedule" |
| **Error — Schedule, no cache** | Fetch fails, first-ever load | Structural empty state with "Tap to Retry" |
| **Error — Standings** | Fetch fails, cache exists | Render cached standings, text dimmed via `--color-text-secondary` (**not** `--color-text-disabled`, which is reserved exclusively for disabled controls per V6); blocking banner: "⚠️ Connection lost. Standings may be out of date. [RETRY]" |
| **Error — Standings, no cache** | Fetch fails, first-ever load | Structural empty state with "Tap to Retry" |
| **Error — Riders** | Fetch fails, cache exists | Silent fallback to cached view; no banner (low time-sensitivity persona) |
| **Error — Riders, no cache** | Fetch fails, first-ever load | Minimal empty state: "Rider profiles unavailable — check your connection [Retry]" |

## Prototyping Tool
**Figma** — confirmed.

## Redraw Required Before Artifact Sign-Off
Initial lo-fi mockups (`desktop.png`, `mobile.png`) contained three regressions caught in review and must be corrected in the next pass:
1. Desktop reintroduced a 4-destination merged dashboard — rejected, violates Decision #5.
2. Both breakpoints showed Remote Time before Local Time — contradicts Decision #2.
3. Mobile day-tabs rendered below the session cards instead of above — violates control-before-content sequencing.

## V7 Status
**Decisions: Closed.** All seven blocking questions resolved and documented above.
**Artifacts: Redraw pending.** Wireframes need the three corrections above before V7 is fully closed with delivered visuals.

**Commit (once redraw is done):** `docs: add V7 low-fidelity prototype decisions and screen-state inventory`
