# ALTITUDE

**Concept:** Altitude — Worlds as a sky you descend through, one altitude of detail at a time.
**Slug:** `altitude` · **Type:** concept prototype, **MOCKUP**, not approved, not built.
**Prototype:** [index.html](index.html) — one self-contained file, no network, no build step.

---

## The one-line idea

Worlds is not a page, it is a **place with height**. The six services are six sectors
of one sky. From 30,000 ft you see only the mood of the house; at 3,000 ft the sectors
unfold into named services with their states and ages; at **ground** you read the fact
itself, where it came from, how old it is, and who may act on it. **Detail is unfolded
by descending, never hidden** — and descending cannot fill in a sector that has never
spoken.

## The main gesture

**Descend.** Two 44px buttons (`↑ Ascend` / `↓ Descend`) are the keyboard path; the
three altitude stops in the rail jump straight to a level; clicking a sector, clicking
the middle of the sky, or swiping on a touchscreen does the same thing with a hand.
One control, and the whole sky recomposes: the drawing, the labels, the rail readout
and the surface below all change together, at their own level of detail.

## The three working interactions

1. **Descend / ascend** (the one that updates several views at once). Moving one
   altitude step recomputes *the sky* (a single ring of words → named sectors with ages
   → the one sector you descended onto), *the sky's written caption*, *the rail readout*
   and *the surface panel* (mood → fact, source and age → the full ground view with the
   two readers, the permissions split and any action). Nothing is withheld at any
   level: the rail says plainly "actions and permissions live at ground level; descend
   to reach them".
2. **Select a service** from the compass chips (or click a sector) — the sky
   highlights that sector, the lens in the middle becomes that service, and the surface
   below is rewritten. This is the second multi-view interaction.
3. **Record the Backups decision** at ground level (3 files could not be copied): keep
   it flagged (safe, first), ask the agent to *prepare* a retry (proposal only), or
   authorise one retry (human only). Recording changes the sky's triangle badge, the
   rail readout, the ground panel and the agent's copy of the fact — together.
4. **Act inside the embedded Home lights app** — toggle any of the 7 lamps in the app
   drawn inside Worlds at ground level, or press **"Simulate can't embed"** for the
   honest fallback. Every lamp change is labelled a drawing and is quarantined from the
   shared fact.

Two quieter behaviours: an honest **re-check** on Files and Weather ("still stale",
"still no answer"), and the fact that **Weather's sector resists descent** — there is
nothing there to unfold, and Worlds says so rather than inventing a card.

## What is embedded

**Home lights** — a 7-lamp panel drawn inside Worlds at ground level, under Worlds' own
accessibility floor (52px targets, keyboard operable, `aria-pressed`, state written as
"On"/"Off"). The owning app is always one link away: **Open in the owning app ↗** →
`https://example.invalid/home-lights` — present both in the ground panel and next to
the "descend to open it" line at 3,000 ft. *"Simulate can't embed"* swaps the panel for
a fallback card that names the reason and keeps the same link.

## Shared facts, two readers

At ground level the surface shows the selected fact **twice, side by side**: *your
copy* and *the agent's copy*. Same sentence, same source, same `heard` timestamp; the
differences are stated explicitly — your private note ("ask Sam about the NAS
permissions") is visibly absent from the agent's copy, and the agent cannot authorise a
retry, only prepare one. A `<details>` panel, "The shared surface — all six facts,
source, age and permissions", lists every fact with its source, freshness and the two
**separate** permission lists (`Agent may` / `Human may`); the rail button opens it from
any altitude.

## Honest states, stated in words

- **stale** — Files. Its arc **stops short of now**, dashed, with the remainder drawn
  as a faint dotted trace: heard 05:14, "3 hours ago". The word, the age and the gap
  travel with the fact to both readers.
- **unknown** — Weather. An **empty outline**, dotted, labelled "never", with the words
  "never answered" in the rail, the chips and the surface. The fact row reads "No
  reading exists" — never a blank or a guess.
- Every status is a written word (`Fine`, `Needs your decision`, `Stale`, `Unknown`)
  **plus a shape** (solid arc / triangle / dashed arc / empty dotted outline). The sky
  itself is `aria-hidden` — the same information is in the rail, the chips and the
  surface, in words.

## Accessibility and responsiveness

- Works at 390px with no page-level horizontal scroll: the stage becomes a single
  column and the sky drops its name/age labels (keeping the status words) instead of
  shrinking text below readability. The captions carry the rest in words.
- 44px minimum targets throughout; skip link first; 2px teal `:focus-visible` ring.
- Every action is keyboard-reachable through labelled buttons — the sky is never the
  only way to do anything — and focus survives re-renders.
- `prefers-reduced-motion`: transitions only inside
  `@media (prefers-reduced-motion: no-preference)`.
- Announcements go to one polite live region, on committed changes only (altitude,
  selection, decisions, demo switches). Aligned with the repository
  [accessibility contract](../../../accessibility/ACCESSIBILITY_CONTRACT.md).

## What a Connect API would need to supply

To render Altitude for real, each service would expose via Connect:

1. **Observations as first-class facts** — stable id, a `status` word from the shared
   vocabulary, one human sentence, and `heard_at`, so age is derived and can be drawn
   as an arc that stops short.
2. **Explicit absence** — an honest *unknown* / *not configured* signal (Weather),
   distinct from an empty value, so the sky can draw an empty outline and refuse to
   unfold it.
3. **Provenance** — a human-readable `source` per fact, and an owning-app link for the
   fallback path.
4. **Per-principal authority** — separate `agent_may` and `human_may` capability lists
   per service, so the two readers can be compared side by side; plus a
   request → approve → dispatch path (at most once, durable SUCCEEDED / FAILED /
   UNKNOWN) for consequential actions.
5. **Embed descriptors** — whether a service can be embedded, the embed entry point,
   and a fallback URL for when it cannot.
6. **A level-of-detail hint** (optional but useful): which facts are safe to summarise
   at a glance and which need their full sentence, so the sky never over-summarises a
   fact into something untrue.

## Notes and limits (what this concept does *not* claim)

- It is a **mockup with fixed sample data** (Tuesday 13 October, 08:14). No service is
  contacted, no fact is fetched, no action is dispatched.
- The embedded lights panel is simulated and does not change the shared fact.
- The decision shows a safe default first; a production build would run "Authorise"
  through the full governed authority flow.
