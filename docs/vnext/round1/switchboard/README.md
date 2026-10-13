# Switchboard (MOCKUP)

**Idea:** Worlds is a telephone exchange. Two operators, the human and the agent, sit on the left; every service is a jack on the right. Whoever is plugged in gets cords that show exactly what they may do at each service.

**Main gesture:** plug in an operator (tap, click, or Tab + Enter). The brass cord redraws to every service: solid = may act, dashed = read only, dotted with "?" = nothing to read.

## Three interactions
1. **Plug in Human / Agent**: redraws all cords, rewrites each jack's cord note, the top summary and the panel's enabled actions.
2. **Resolve Backups** (**updates several views at once**): Retry (human or agent) takes 3 failed files to 1; Skip (human only) clears the last. Each step changes the Backups jack (state word and line), the summary, the Service panel, the Shared facts tab and the change log. As Agent, Skip is disabled with a written reason.
3. **Embedded apps**: toggle 7 bulbs (the jack's "n on" line updates) or play/pause/skip the media player.

## Embedded
Media library (fake player) and Home lights (7 bulbs) open inside the lower panel in a framed "EMBEDDED" window, each with "Open in the owning app" linking to `https://example.invalid/...`. Other services show a "not embeddable" frame with the same fallback.

## Honest states
Files is **Stale** ("last heard 3 hours ago, may be out of date"). Weather is **Unknown** ("never answered", a "?" cord end). "Ask for a re-check" returns no reading and says so. State is always a written badge, never colour alone.

## Shared facts
The "Shared facts" tab lists every fact with source, freshness, and separate **Agent may** / **Human may** lines.

## What a Connect API would need to supply
- Per service: state, last-heard timestamp, one-line summary.
- Per fact: key, value, source id, observed-at, agent and human permission sets (including "no reading exists").
- Actions with declared actor scope (retry vs skip) and an audit entry in the response.
- An embed descriptor (URL, allowed origins, sandbox flags, or "not embeddable") plus the owning-app URL.
- A change feed so cords and states update live.

Sample data is invented; time is fixed at Tuesday 13 October, 08:14. Note: authored without a browser or JS runtime available, so it was syntax-balance-checked only, not run.
