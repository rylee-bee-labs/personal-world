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
