# Strata (MOCKUP)

**Idea:** Worlds is a cross-section of ground. Services sit in layers by how recently they were heard: surface (minutes), sediment (hours, stale), bedrock (never heard). A single depth slider digs through *what you can trust*: the facts, their sources and age, and who may act on them.

**Main gesture:** dig. Drag the depth slider (or use arrow keys on it) and every stone recomposes from "now" to "sources and age" to "who may".

## Three interactions
1. **Dig depth** (range slider, keyboard friendly): all six stones change content at once: summary line, source, freshness bar, then separate Agent may / Human may lines. This is the shared-facts view; there is no separate page.
2. **Resolve Backups** (**updates several views at once**): excavate the Backups stone, then Retry (human or agent) takes 3 failed files to 1; Skip (human only) clears the last. The stone, its tag, the readout ("1 thing needs a human" to "Nothing needs a human"), the dug pit and the change log all change. Needs-human stones sort to the top of their layer.
3. **Excavate a stone / act as Human or Agent**: tapping a stone opens a pit inline, pushing the layers below down. Media (fake player) and Home lights (7 bulbs) are embedded in it. The "Acting as" switch changes permissions, e.g. Skip becomes disabled with a written reason for the agent.

## Embedded
Media library and Home lights embed in the pit in an "EMBEDDED" frame; every pit has "Open in the owning app" to `https://example.invalid/...`. Others show "not embeddable" plus the same link.

## Honest states
Files sits in **Sediment**, tagged "Stale", dashed border, "last heard 3 hours ago". Weather sits in **Bedrock**, tagged "Unknown", dotted border, empty freshness bar, "never answered". "Ask for a re-check" returns no reading and says so.

## What a Connect API would need to supply
- Per service: state, last-heard timestamp (to place it in a layer), one-line summary.
- Per fact: key, value, source id, observed-at, agent and human permission sets.
- Actions with declared actor scope and an audit entry.
- An embed descriptor (or "not embeddable") plus owning-app URL.
- A change feed so stones move between layers live as freshness changes.

Sample data is invented; time is fixed at Tuesday 13 October, 08:14. Note: authored without a browser or JS runtime available, so it was syntax-balance-checked only, not run.
