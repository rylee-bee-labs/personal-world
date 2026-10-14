# Worlds design v2: refinement audit (find every missing piece)

## Decisions in force
- 2026-10-13 Rylee: "I love this. Let's send back for one round of refinement and to get every additional piece down and all the reactive elements we might need. Please. I'd like this to be a little robust and polished along with our whimsical snail"
- Protect the look, Dot, the warmth and delight; copy stays terse warmth (spec section "Voice: terse warmth").

Read `docs/vnext/worlds-design.md` (the spec), `docs/vnext/prototype/index.html` (the prototype; open and read its JS), `docs/vnext/prototype/art/` (the art that exists), `docs/vnext/connect/connect-contract.md`, `docs/vnext/house/`, `docs/vnext/field-guide/`, and `docs/vnext/fixture/fixture.json`.

Write `docs/vnext/refine-audit.md`: a complete, prioritised punch list for ONE refinement round, in four sections. Each item: what, where (screen/component, spec section), why, the concrete fix, and size (S/M/L).
1. **Missing pieces**: screens, states, components or flows the spec names but the prototype lacks or stubs, and things a daily-use app needs that neither has (e.g. search/filter of slips, sorting, notifications/inbox for needs-you, history/activity log, multiple services of the same kind, editing a credential reference, re-test, renaming, help/about, first-run replay, undo after disconnect, empty states for every list).
2. **Reactive elements**: every live behaviour the app should have and whether it works: live feed tick, freshness ageing to stale, unknown on silence, propagation of one change to every view, optimistic actions with settle and rollback on failure, loading/skeletons, progress, toasts and the live region, offline and reconnect, background retries with backoff, counters/badges updating, transitions between states (and their reduced-motion forms), pause/resume.
3. **Robustness**: edge cases and failure paths: invalid input, double clicks, rapid toggling, every Connect step failing, embed refused, very long names, many services (20+), zero facts, timezone/relative-time correctness, state persistence across reload (local only), keyboard traps, focus restoration, screen-reader announcements, contrast in all four house modes, 200% text, 320px width.
4. **Polish**: visual consistency, spacing rhythm on the token ladder, alignment, hierarchy, motion timing, icon/art placement and sizing, where Dot appears (and where Dot should stay quiet), art gaps (list any NEW art needed, max 6 pieces, with a one-line description each, in Dot's existing style).
Be exhaustive but concrete; no generic advice. Write only that file. Do not push.
