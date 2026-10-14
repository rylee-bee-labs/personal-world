# Worlds vNext — creativity round 1: one bold concept (identical brief for every model)

Four model families get this exact brief and each makes ONE concept. Rylee compares them side by side to see which model is most creative. Be bold; do not play safe.

## Context
Worlds is a person's home for the independent services they run (media library, backups, home lights, files, a project board, weather). An earlier round produced three tidy, conventional dashboards (a shoreline, a harbor, a daily note). Rylee found them **too unimaginative**. She wants:
- a **genuinely reactive** interface: things respond, move, recompose and update live as you interact;
- **shared factual surfaces**: the same facts an AI agent sees are visible to the human, with their source, freshness and who may act on them (agent vs human permissions shown separately);
- **apps inside Worlds**: a supported external app can open *inside* Worlds (an embedded panel/window/card), with a fallback link out when embedding is not possible.

Invent the information architecture, navigation, main gesture and composition. Not a sidebar admin dashboard, not a card grid with a header. Surprise her.

## Hard requirements (a working prototype, not a picture)
Make ONE self-contained file `docs/vnext/round1/<your-slug>/index.html` (inline CSS and JS, no network, no build step, no external fonts or images; inline SVG/canvas is fine) plus `README.md`.
1. **Three working interactions**, done in real JS. At least one interaction must update **several views at once** (for example, resolving one item changes a map, a list and a summary together).
2. **An embedded demo app** (e.g. a fake media player or lights panel drawn inside Worlds) with a visible **fallback** "open in the owning app" link (use `https://example.invalid/...`).
3. **Shared facts panel/mode**: show which facts the agent can see, their source and freshness, and separate *agent may* / *human may* permissions.
4. **Honest states**: at least one service is **stale** and one is **unknown**, said in words, never colour alone. Nothing pretends to be a real reading.
5. **Phone and keyboard**: works at 390px wide with no horizontal scroll and 44px touch targets; every interaction is reachable by keyboard with visible focus; respects `prefers-reduced-motion`.
6. A visible **MOCKUP** label (it is a concept prototype, not approved, not built).

Use this fixed sample data so concepts compare fairly: services Media library (fine, 2 new episodes), Backups (needs a human decision: 3 files could not be copied), Home lights (fine, 7 of 7 reachable), Files (stale: last heard 3 hours ago), Project board (fine, 3 cards due this week), Weather (unknown: never answered). The time is Tuesday 13 October, 08:14.

## README.md
Concept name, the one-line idea, the main gesture, the three interactions (and which one updates several views), what is embedded, and what a Connect API would need to supply.

## Rules
Work only in your clone. Do not push, deploy, or touch src/ or ui/. When done, list the files you created.

## THIS RUN: concepts 2 and 3 of your three
You already made concept 1 (its README is pasted below). Now make TWO MORE concepts, each in its own folder `docs/vnext/round1/<slug>/` with its own `index.html` and `README.md`. All three of your concepts must be **clearly different from each other**: different information architecture, navigation, main gesture, and composition. Rylee is measuring your range, not one lucky hit. Every hard requirement above applies to each concept.

### Your concept 1 README

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


## Decisions in force
- 2026-10-13 Rylee: "Can we have a round one where each including codex generates an example ... Then we know if one is more creative or not"; "Maybe it should be each does 2 or 3?"
