# Worlds vNext — concept brief v2 (round 2): Worlds is a shell that real services plug into

Status: MOCKUP brief. Nothing produced from it is approved.

## Decisions in force (quoted)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer."
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page. maybe have them develop a mini design.md for each build? I don't hate them they're just not quite.... ready for a bigger test"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"
- Owner feedback on #322 (comment 6153184069): genuinely reactive UI; shared factual surfaces for agents and humans; supported external apps usable inside Worlds; static images do not count as prototypes.

## The one thing round 1 missed
Round 1 made beautiful single pages. Worlds is not a page. **Worlds is a shell (a frame) that real, independent services plug into.** Each service owns its data, its actions and its own UI. Worlds discovers it, connects it with a credential *reference*, composes its facts, frames its app, and lets go of it cleanly. Your concept must make that integration visible and working.

## Integration requirements (every concept, all working in the prototype)
1. **Plug-in, live.** The prototype starts with 2 connected services and lets the user add a 3rd through the Connect lifecycle: *discover → credential reference (never a secret value) → safe test → preview of the facts/capabilities it offers → choose what to publish → it appears in Worlds*. Use the lifecycle shape from the Connect contract draft (#329), not its details.
2. **Composition.** At least 3 services from `docs/vnext/fixture/fixture.json` (use its ids, names and states verbatim; add nothing it does not contain). Each owns its own data and actions; Worlds composes them.
3. **Embedded app frame.** Open one service's own UI *inside* Worlds (drawn as a framed app, iframe/ext-app style), with a visible fallback "open in the owning app" link (`https://example.invalid/...`). Worlds stays the frame: its navigation, back and status keep working around the embedded app.
4. **One fact, many places.** One change in one service (it goes stale, or a need appears) updates, at once, the home, that service's own surface, and the agent-facts view. The agent-facts view shows each fact's source and freshness and, separately, *who can see* and *who can act* (agent vs human).
5. **Governed action hands off.** A decision is never made in Worlds; it links/hands off to the owning system (Project Home for approvals, the service itself otherwise).
6. **Disconnect.** Remove a service; Worlds still works and says plainly what is gone.

## Floors (from round 1, unchanged)
Phone at 390px with no sideways scroll and 44px targets; every interaction reachable by keyboard with visible focus; `prefers-reduced-motion` honoured; stale and unknown said in words, never colour alone; a visible MOCKUP label; no restyle of the old FdApp; nothing presented as a real reading.

## Prompt guidance applied (rylee-bee-workspace/media_files/designs/_index/DESIGN-QUALITY-GUIDE.md)
- "quality comes from decision coverage, not word count": each concept pins its decisions in DESIGN.md before any code.
- Bind to a design system (lever 1): each concept has an assigned design system; do not invent values outside it.
- Craft rules injected (lever 3): `docs/vnext/od-pack/` holds OpenDesign's craft rulebooks (anti-ai-slop, color, typography, laws-of-ux, state-coverage, accessibility-baseline, animation-discipline). Follow them.
- The 4 craft questions (5a, 9-12): single job, the characteristic artifact as the hero ("the hero is a thesis"), ONE signature element, a reference lock of 1-2 named real designs (take mechanics, not palettes).
- Never one-shot (section 6): DESIGN.md → critique by a different engine (≤5 gaps) → fix → build → screenshot check.
- Stack rule (section 4): single self-contained HTML, hand-tuned CSS, vanilla JS; no frameworks, no CDN, no external fonts or images.
- Exclude list: emoji icons, lorem, invented metrics, hero gradients, Tailwind-indigo accent, hue-only status, motion on by default.

## Anti-repeat list (do not reproduce)
Past library: Pip-Boy/CRT terminals, cyberdecks, galaxy/star maps, grimoires, dungeon crawlers, tech trees, Horizon "Focus" HUDs, color-swap templates. Round 1 (all twelve): field atlas map, patchbay cables, radio-frequency dial, time-scrubbing now-line, radio band stations, switchboard, woven threads/loom, geological strata, plain-text desk, split "reach" surface, altitude dial, orrery of orbits. An evolution of your own round-1 concept is allowed if it becomes a real integrated shell; a copy of anyone else's is not.

## Mechanics references (from docs/vnext/design-research.md)
Homarr 2.0 (reactive widgets that reuse one integration for UI and assistant), Appsmith (reactive bindings), Dashy (apps opened in place), Home Assistant custom cards (shared state subscriptions), MCP ext-apps (sandboxed tool UIs). Borrow mechanics; say in DESIGN.md what you borrow and what you deliberately do differently.

## DESIGN.md (write FIRST, about one page, these headings)
1. Metaphor in one line, and the main gesture.
2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
3. Navigation between Worlds and a service (phone and desktop).
4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
5. The agent/human shared-facts model (source, freshness, who sees, who acts).
6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
9. Signature element, hero thesis, reference lock.
10. Risks and UNKNOWNs.

## Output contract (the build)
`docs/vnext/round2/<concept-id>/index.html` (single self-contained file) beside its `DESIGN.md`. The prototype must demonstrate requirements 1-6 with real JS. Label it MOCKUP.
