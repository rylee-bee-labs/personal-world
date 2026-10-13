# The Quiet Frequency

**One-line idea:** Worlds is a responsive signal instrument: service events bend a live compass, while a selected signal retunes the evidence beside it.

**Main gesture:** Tune the instrument by selecting a signal/source in the service strip; the evidence readout and shared-fact sheet recompose together.

## Three interactions

1. Select a service signal to update its status/evidence, provenance, shared fact, freshness, and separate permissions at once. This updates several views.
2. Mark the backup decision reviewed; the attention wording, instrument summary, and backup signal label update together. It only records a mock review.
3. Show or hide the shared-facts surface. The embedded demo has an additional simulated play/pause control.

## Embedded app

A small fictional media listening room runs inside Worlds. Playback is only simulated. “Open in the owning app” falls back to `https://example.invalid/media-library`.

## Connect API needs

A Connect API would supply service identity, provider-neutral status and summary, typed facts with source and observation time, freshness thresholds and explicit unknown reasons, stable shared fact IDs for human and agent surfaces, distinct agent and human permissions, reviewable attention state, and an optional supported embed descriptor with capability limits and an owning-app fallback URL. It should identify observed versus stale versus never-known data without inventing values.

## Prototype notes

Offline single-file mockup with the fixed six-service sample. Time shown: Tuesday 13 October, 08:14. **MOCKUP**; no real readings or actions are connected.
