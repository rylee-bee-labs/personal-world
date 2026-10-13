# NOWLINE

**Concept:** Nowline — Worlds as one instrument you sweep across the morning.
**Slug:** `nowline` · **Type:** concept prototype, **MOCKUP**, not approved, not built.
**Prototype:** [index.html](index.html) — one self-contained file, no network, no build step.

---

## The one-line idea

Worlds doesn't hand you a page of tiles. It hands you a **strip chart of when it
last heard from each service**, and a single hand you sweep across the morning:
everything left of the hand is what Worlds knew *as of that moment*, everything
right of it simply hadn't arrived yet — and nothing is ever invented to make the
chart look fuller.

Freshness stops being a badge and becomes the **navigation axis**. Time is the
information architecture; the hand is the gesture.

## The main gesture

**The sweep.** One hand (a slider / the `as of` line) moves across 05:00–08:14.
Sweeping it re-reads the *whole* world at once. Sweeping back makes facts that
hadn't arrived yet go hollow, and can turn a fact that is stale now into a fresh
one then; sweeping past the present adds nothing, because Worlds will not
manufacture a reading. Keyboard: the hand is a native range control (`←` `→`,
`Home`, `End`), and four preset chips jump to Now, the backup failure, this
morning, and first light.

## The three working interactions

1. **Sweep the as-of hand** — *(this is the one that updates several views at
   once)*. Moving the hand recomposes **the strip chart**, **the read** (plain-
   language summary), **the ledger**, the **count chips**, and the **shared
   surface** together, from one recomputed state. Example: sweeping from 08:14
   back to 06:00 turns Files from `stale` to `fresh`, makes three services read
   `not yet heard at this reading`, and rewrites the summary sentence — without
   any new contact being claimed.
2. **Record the Backups decision** — Backups is `needs your decision` (3 files
   could not be copied). Opening it offers three outcomes — keep it flagged
   (the safe default), ask the agent to *prepare* a retry (proposal only), or
   authorise the retry (human-only, consequential). Recording one updates the
   Backups lane, the ledger, the read and the shared permissions view at once.
3. **Act inside the embedded Home lights app** — toggle any of 7 lamps in the
   panel drawn inside Worlds, or press **“Simulate can’t embed”** to see the
   honest fallback state. Every lamp change is labelled as the demo it is and is
   deliberately **quarantined from the shared fact** — the embedded demo cannot
   rewrite what Worlds says it knows.

Two quieter behaviours support these: the **You / Agent lens** switch (shows that
the agent's surface is exactly yours, minus the note you keep private), and an
honest **re-check** on Files and Weather that returns “still stale” / “still
unknown — no answer” rather than a fabricated reading.

## What is embedded

**Home lights** — a 7-lamp panel rendered inside Worlds under Worlds' own
accessibility floor (44px targets, keyboard operable, `aria-pressed` state).
The owning app is always one link away: **Open in the owning app ↗**
→ `https://example.invalid/home-lights`. The *“can’t embed”* mode swaps the panel
for a fallback card that names the reason and keeps the same link, modelling the
required behaviour when embedding is not possible.

## Honest states (stated in words, never colour alone)

- **stale** — Files, “last heard 3 hours ago (05:14)” at the present reading.
- **unknown** — Weather, “never answered — nothing is known”; a re-check still
  returns unknown.
- Every status also has a written word (`fine`, `stale`, `needs your decision`,
  `unknown`, `not yet heard`) plus a shape (solid / dashed / dotted / barred),
  and the footer says plainly that nothing on the page is a real reading.

## Accessibility and responsiveness

- Works at 390px with no page-level horizontal scroll (positions are
  percentage-based; the mobile sheet stays under half the viewport so it is
  legitimately non-modal).
- 44px minimum targets throughout; skip link first; visible 2px teal focus ring.
- Every interaction is keyboard-reachable; the non-modal panel moves focus in,
  closes on `Escape`, and returns focus to the invoking lane (or a stable
  fallback when that lane was re-rendered).
- `prefers-reduced-motion` is respected — all transitions are opt-in inside
  `@media (prefers-reduced-motion: no-preference)`.
- Live-region announcements are batched and only fire on committed changes, not
  while dragging. Aligned with the repository
  [accessibility contract](../../../../docs/accessibility/ACCESSIBILITY_CONTRACT.md).

## What a Connect API would need to supply

To render Nowline for real, each service would expose via Connect:

1. **Observations as first-class facts** — for each service: a stable id, a
   `status` word from the shared vocabulary, a short human summary, and
   `heard_at` (the moment the fact was true), so freshness is derived, never
   asserted.
2. **Explicit absence** — a way to say *unknown* and *not configured* honestly
   (Weather's “never answered”) rather than a missing row or a guessed value.
3. **Provenance** — a human-readable `source` and, where useful, a link to the
   owning app for the fallback path.
4. **Per-principal authority** — separate `agent_may` and `human_may` capability
   lists per service, and for consequential actions a request/approve/dispatch
   path (at most once) with a durable outcome of SUCCEEDED, FAILED or UNKNOWN.
5. **Embed descriptors** — whether a service can be embedded, the embed entry
   point, and a fallback URL for when it cannot.
6. **A freshness policy** — per-service freshness windows, so “stale” means the
   same thing in the human's reading and the agent's.

## Notes and limits (what this concept does *not* claim)

- It is a **mockup with fixed sample data** (Tuesday 13 October, 08:14). No real
  service is contacted; no action is dispatched.
- The embedded lights demo is simulated and explicitly does not change the
  shared fact.
- The decision sheet is non-modal provenance; its dangerous option starts on the
  safe default. A production build would route the “authorise” path through the
  full governed authority flow (request → approve → dispatch at most once).
