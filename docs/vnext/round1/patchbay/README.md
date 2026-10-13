# PATCHBAY

**Concept:** Patchbay — Worlds as a signal board you re-wire each morning.
**Slug:** `patchbay` · **Type:** concept prototype, **MOCKUP**, not approved, not built.
**Prototype:** [index.html](index.html) — one self-contained file, no network, no build step.

---

## The one-line idea

Worlds is not a page of tiles, it is a **board of sockets and jacks**. Every service
is a socket, every reading can be **patched** to *you*, to the *agent*, or to
*nobody*. Re-wiring the board changes **who is told** — it never changes **what is
true**. Routing is a promise about attention, not a claim about the world, and the
shared surface below the board proves it: one list of facts, two readers.

## The main gesture

**Pull a cord.** Drag the plug on a service to the *Your morning read* or
*Agent's context* jack and the cord snaps into place. The switch equivalent — two
44px buttons on every socket — is the primary control and the only one keyboard
needs: `Tab` to a socket's switches, `Enter` to throw them. The cord canvas is the
picture of that same wiring, redrawn live.

## The three working interactions

1. **Re-route a service** (the one that updates several views at once). Throwing one
   switch recomputes *the cord canvas*, *the "Your morning read" list*, *the
   "Agent's context" list*, *the summary strip*, *the cord counts on each jack*, and
   *the "Who is told" line on every fact of the shared surface*. Example: unpatched
   from the agent, Files disappears from the agent's list and the summary drops to
   "4 cords to the agent" — while the fact itself, its source and its 05:14 timestamp
   stay exactly as they were.
2. **Record the Backups decision.** Backups is `needs your decision` (3 files could
   not be copied). Three outcomes: keep it flagged (the safe default, first and
   focused by default), ask the agent to *prepare* a retry (a proposal only), or
   authorise the retry (human-only, consequential). The record updates the bench, the
   socket's state, the summary, the cord the agent reads, and the fact on the shared
   surface — together.
3. **Act inside the embedded Home lights app** — toggle any of the 7 lamps in the
   panel drawn *inside* Worlds, or press **"Simulate can't embed"** to see the honest
   fallback. Every lamp change is labelled as the demo it is and is deliberately
   **quarantined from the shared fact**: the embedded demo cannot rewrite what Worlds
   says it knows.

Two quieter behaviours support these: a **refusal** — trying to patch Weather, which
has never answered, is answered with "nothing to route: no reading exists", and no
cord is drawn — and an honest **re-check** on Files and Weather that returns "still
stale" / "still no answer" instead of a fabricated reading.

## What is embedded

**Home lights** — a 7-lamp panel drawn inside Worlds in the bench, under Worlds' own
accessibility floor (52px targets, keyboard operable, `aria-pressed` state, state
written as "On"/"Off", not colour alone). The owning app is always one link away:
**Open in the owning app ↗** → `https://example.invalid/home-lights`. The *"Simulate
can't embed"* switch replaces the panel with a fallback card that names the reason and
keeps the same link, modelling the required behaviour when embedding is impossible.

## Honest states, stated in words

- **stale** — Files: "Last heard 3 hours ago", heard 05:14, and a re-check that says
  "still stale"; its fact carries the age wherever it is routed, and the agent's copy
  is marked `stale: the agent is told the age, not just the value`.
- **unknown** — Weather: "Never answered". Its socket offers no cord and says
  "no reading exists, so there is nothing to patch"; the bench will not render a value.
- Every status is a written word (`Fine`, `Needs your decision`, `Stale`, `Unknown`)
  **plus a shape** (solid square / triangle / dashed / dotted ring) — never colour
  alone. The footer states that nothing on the page is a real reading.

## Accessibility and responsiveness

- Works at 390px with no page-level horizontal scroll: below 900px the board folds
  into a list — same sockets, same switches, words instead of cords.
- 44px minimum targets (sockets, switches, lamps, decision buttons); skip link first;
  2px teal `:focus-visible` ring.
- Every action is keyboard-reachable; focus survives re-renders (each control carries
  a stable key and is re-focused after the view updates), so a committed change never
  drops the user at the top of the page.
- `prefers-reduced-motion`: all transitions live inside
  `@media (prefers-reduced-motion: no-preference)`.
- Announcements are batched into one polite live region on *committed* changes, not
  while dragging. Aligned with the repository
  [accessibility contract](../../../accessibility/ACCESSIBILITY_CONTRACT.md).

## What a Connect API would need to supply

To render Patchbay for real, each service would expose via Connect:

1. **Observations as first-class facts** — a stable id, a `status` word from the shared
   vocabulary, a short human sentence, and `heard_at` (when the fact was true), so
   freshness is *derived*, never asserted.
2. **Explicit absence** — a way to say *unknown* and *not configured* honestly
   (Weather's "never answered") rather than a missing row or a guessed value. The
   board must be able to refuse to draw a cord to a fact that does not exist.
3. **Provenance** — a human-readable `source` per fact, and an owning-app link for the
   fallback path.
4. **Per-principal authority** — separate `agent_may` and `human_may` capability lists
   per service, plus a request → approve → dispatch path for consequential actions
   (dispatch at most once, durable outcome of SUCCEEDED / FAILED / UNKNOWN).
5. **Embed descriptors** — whether a service can be embedded, its embed entry point,
   and a fallback URL for when it cannot.
6. **A freshness policy** — per-service windows, so "stale" means the same thing on the
   board, in the human's reading, and in the agent's context.

## Notes and limits (what this concept does *not* claim)

- It is a **mockup with fixed sample data** (Tuesday 13 October, 08:14). No service is
  contacted, no fact is fetched, no action is dispatched.
- The embedded lights panel is simulated and explicitly does not change the shared fact.
- The decision is non-modal provenance; its dangerous option starts on the safe default.
  A production build would route "Authorise" through the full governed authority flow
  (request → approve → dispatch at most once).
