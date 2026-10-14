# Worlds design: refinement round 1, build it

## Decisions in force
- 2026-10-13 Rylee: "I love this. Let's send back for one round of refinement and to get every additional piece down and all the reactive elements we might need. Please. I'd like this to be a little robust and polished along with our whimsical snail"
- Protect the look, Dot, the warmth and delight (spec §1). Copy stays terse warmth (spec "Voice: terse warmth"); new strings follow it.

## Inputs
- `docs/vnext/worlds-design.md`: the spec, now with **"Refinement round 1: checklist"** (118 items) and **"Reactive components"**.
- `docs/vnext/prototype/index.html`: the current prototype. Its art is referenced through one map of names to `art/<name>.webp` paths; keep using that map (Claude embeds the images as data URIs afterwards, so the file stays one self-contained file). New art is in `docs/vnext/prototype/art/`: `dot-peek`, `dot-tucked`, `dot-settled`, `icon-generic`, `scene-quiet`, `empty-ledger`, plus every earlier piece (Dot poses now at 2x: keep display sizes the same). Add new keys to the same map.
- The fixture `docs/vnext/fixture/fixture.json`, the house tokens `docs/vnext/house/tokens.css`.

## Do
1. Implement the **Reactive components** exactly as specified (LiveFactRow, FreshnessBadge, ServiceCard states, DotMood, Counter/badge, ChangeRipple, LiveTick, Toast + live region, OptimisticAction, Skeleton) as reusable functions, and use them everywhere the spec says, so behaviour is consistent across screens.
2. Work through the **checklist**, top to bottom by priority. Implement every item you can in one self-contained file; tick each one in the spec (`- [x]`) as you finish it, and for anything you cannot finish, leave it unticked with a short reason after it. Prefer finishing more S and M items over half-finishing an L.
3. Robust: no script errors; guard double clicks and rapid toggles; every Connect step's failure path works; long names and 20+ services don't break layout; relative times correct and ticking; local persistence across reload (localStorage, wrapped in try/catch); focus restored after dialogs and route changes; one polite live region; 44px targets; no layout shift on state changes; contrast holds in dark and light.
4. Polished: spacing on the token ladder, one type scale, consistent icon sizes, motion timings from the spec's named durations with reduced-motion forms.
5. Keep the checker contract: `#skip-welcome`, `#add-service`, `#disconnect` (visible on the first screen for the selected service; confirm button labelled exactly "Disconnect"), `#simulate-change`, `#live-toggle`, a visible MOCKUP label. Add `#undo` for the undo after disconnect.
Write only `docs/vnext/prototype/index.html` and the ticked checklist in `docs/vnext/worlds-design.md`. Do not push.
