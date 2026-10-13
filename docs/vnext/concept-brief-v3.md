# Worlds vNext — concept brief v3 (the real rounds)

Status: MOCKUP brief. Nothing produced from it is approved. **Everything in `concept-brief-v2.md` still applies** (integration requirements 1-6, floors, prompt guidance, output contract). This brief adds to it.

## Decisions in force (quoted)
- 2026-10-13 Rylee, choosing from round 2: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

## The direction: concept C ("Service cabinet")
Read `docs/vnext/reference/direction-C-DESIGN.md` (and look at `direction-C-index.html`). Rylee liked its direction: a real application shell (persistent Home / Connect / Memory / Settings), services as clearly owned, labelled places, the owning app docked beside a shared fact card, honest states, calm structure. **Use C as the starting direction, do not clone it.** Every concept's DESIGN.md must say in one paragraph how it differs from C, and what it keeps.

## What "the real rounds" add
1. **A real application, something she would use every day.** Real navigation, a settings screen, a first-run / empty state, an error state (a service that fails its test or drops off), real copy from the fixture (never lorem), sensible defaults.
2. **Whimsy and warmth, laced in, not a theme park.** Small, specific, delightful moments: kind micro-copy, a tiny illustration or character accent drawn in inline SVG, a gentle motion on a state change, a satisfying "connected" moment. Warmth lives in the details, the structure stays calm and usable. Respect `prefers-reduced-motion`. Status is never colour-only.
3. **Desktop AND mobile, both designed on purpose.** Two first-class layouts, not one stretched: at 1440px wide and at 390px wide (a tablet layout around 768px is a bonus). Mobile has its own navigation pattern (for example a bottom bar or a sheet) and its own reading order; it is not a squeezed desktop. No sideways scroll at 390px; 44px targets.
4. **Reactive.** A simulated live feed from the fixture: a gentle tick (every few seconds) or event stream that changes freshness ("checked 2s ago" → "12s ago"), occasionally moves a service to stale, and updates every view that shows it without a reload. Actions give instant optimistic feedback and settle visibly. Transitions between states are visible (and calm under reduced motion). Provide a visible pause/resume for the live feed.

## Checker contract (an automated script will look for these)
- `#add-service` starts the Connect flow (discover → credential reference → test → preview → publish); stepping buttons are real `<button>`s with clear labels.
- `#disconnect` is **visible on the first screen** for the currently selected service (not hidden until something is opened), and asks for confirmation with a button labelled "Disconnect".
- `#simulate-change` triggers one fixture change that updates Home, the service surface and the agent-facts view at once.
- `#live-toggle` pauses/resumes the live feed; the feed is ON by default (respecting reduced motion for animation, not for data).
- A visible MOCKUP label.

## Anti-repeat
Old library (Pip-Boy/CRT, cyberdecks, star/galaxy maps, grimoires, dungeons, tech trees, Focus HUDs, colour swaps); round 1 (field atlas, patchbay, radio dial, now-line, radio band, switchboard, loom, strata, text desk, split reach, altitude, orrery); round 2 except C (brutalist service ledger, hand-drawn triage chart, dithered contact sheet, swiss service stage, HUD instrument desk, sentence-first home, newsroom night wire, hall of doors, key ring, editorial ledger of facts, daily edition). You may borrow a good mechanism; never the look.

## Your OpenDesign starting point
Each concept has an assigned OpenDesign skill and design system (files in `docs/vnext/od-pack/`). Let them shape the concept; say how in DESIGN.md.
