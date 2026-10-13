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
