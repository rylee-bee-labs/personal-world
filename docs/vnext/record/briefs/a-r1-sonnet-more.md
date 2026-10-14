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

# Orrery (MOCKUP)

**Idea:** your services are bodies in orbit around you; the closer one orbits, the more it needs a human, and whatever you pull to the centre becomes the whole workspace.

**Main gesture:** pull a body into the centre (tap, click, or Tab + Enter). The rest re-orbit by urgency with animated moves.

## Three interactions
1. **Pull to centre**: recomposes the orbit, the detail panel, the log, and the aria labels.
2. **Resolve Backups** (**updates several views at once**): Retry (agent or human) takes 3 failed files to 1; Skip (human only) clears the last. Each step changes the orbit position and colour state, the top summary ("1 thing needs you" to "Nothing needs you"), the panel and the change log.
3. **Acting as Human / Agent**: the same screen with permissions enforced. As Agent, Skip is disabled with a written reason, and orbit captions switch to "agent sees: …". Also interactive: the embedded lights (toggle 7 bulbs; the orbit caption updates) and media player.

## Embedded
Media (fake player) and Home lights (7 bulbs) open inside Worlds in a window frame with an "Open in the owning app" link to `https://example.invalid/...`. Other services show a "not embeddable" frame with the same fallback.

## Honest states
Files is "Stale" (dashed ring, written "last heard 3 hours ago"); Weather is "Unknown" (dotted ring, "?", "never answered"). "Ask for a re-check" does not invent a reading.

## Shared facts
The "Shared facts" tab lists each fact with its source, freshness, and separate **Agent may** / **Human may** lines.

## What a Connect API would need to supply
- Per service: state (fine / needs-human / stale / unknown), last-heard timestamp, and a one-line summary.
- Per fact: key, value, source id, observed-at, and agent and human permission sets.
- Actions with declared actor scope (retry vs skip), with an audit entry returned.
- An embed descriptor (embed URL, allowed origins, sandbox flags, or "not embeddable") plus the owning-app URL for the fallback.
- A change feed so orbits recompose live.

Everything here is invented sample data; time is fixed at Tuesday 13 October, 08:14.


## Decisions in force
- 2026-10-13 Rylee: "Can we have a round one where each including codex generates an example ... Then we know if one is more creative or not"; "Maybe it should be each does 2 or 3?"
