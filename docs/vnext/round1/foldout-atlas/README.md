# Foldout Atlas

**One-line idea:** Your services occupy an invented atlas of places, with evidence that unfolds into the selected place and changes as the map is explored.

**Main gesture:** Choose a place on the map; its field note, shared fact overlay, and map annotation change together.

## Three interactions

1. Select a map place to update the open service leaf, shared fact/source/freshness/permissions, and map annotation. This is the multi-view update.
2. Trace or clear a suggested route across the invented map.
3. Mark the backup exception reviewed; its map annotation, service note, and map label update. This records only a mock review. The embedded demo also has simulated play/pause.

## Embedded app

A fictional Media library listening booth is docked as an app window within the atlas. It does not play audio. Its visible fallback, “Open in the owning app,” points to `https://example.invalid/media-library`.

## Connect API needs

Connect would provide provider-neutral service identities, current summaries, explicit status and unknown reasons, facts with source and observation time, freshness rules, the same stable facts for human and agent views, separate permissions, bounded review state, and optional supported embed metadata plus a fallback URL. The atlas coordinates are decoration, never provider location data; a real map placement would require an explicitly supplied semantic location and appropriate privacy boundary.

## Prototype notes

Offline, single-file mockup using the fixed six-service sample. Time shown: Tuesday 13 October, 08:14. Files is stale and Weather is unknown because it never answered. **MOCKUP**; no real services or app are connected.
