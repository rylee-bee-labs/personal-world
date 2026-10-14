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

# Service Loom

**One-line idea:** Your independent services are threads in one living weave; follow a thread and the shared work surface recomposes around it.

**Main gesture:** Select a service thread. Its summary, detail, provenance, freshness, and shared-facts record update together in place.

## Three interactions

1. **Select a thread** in the loom to recompose the selected-service view, source/freshness, agent/human permissions, and summary together. This is the multi-view update.
2. **Mark the backup issue reviewed** to update the attention area, backup thread text, and whole-loom summary together. This records a mock review only; it does not retry or alter files.
3. **Toggle the shared-facts panel** to reveal or fold the shared agent/human facts view. The embedded demo also has a play/pause control as a secondary working interaction.

## Embedded app

A fictional media player panel is drawn inside Worlds for the Media library service. Its visible “Open in the owning app” fallback points at `https://example.invalid/media-library`. Playback is simulated; nothing is fetched or played.

## Connect API needs

A Connect API would supply a provider-neutral service identity and display name; status vocabulary and human-readable summary; typed facts with source/provenance, observation time, freshness/stale threshold, and explicit unknown reason; the exact same fact payload or stable fact IDs for human and agent surfaces; separate agent and human capability permissions; attention items and safe review state; and an optional supported embed descriptor (origin/path policy, title, sizing, and capability limits) plus an owning-app fallback URL. It must not imply an unavailable value or elevate a saved request into permission to act.

## Prototype notes

All six services use the fixed synthetic sample from the brief and the page labels them mock data. The displayed time is Tuesday 13 October, 08:14. Files is explicitly stale; Weather is explicitly unknown because it never answered. The page is self-contained, offline, and marked **MOCKUP**.


## Decisions in force
- 2026-10-13 Rylee: "Can we have a round one where each including codex generates an example ... Then we know if one is more creative or not"; "Maybe it should be each does 2 or 3?"
