# Worlds vNext real round: critique concept v4065's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v4065/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v4065

# v4065 — Annotated plates (MOCKUP)

## 1. Metaphor in one line, and the main gesture.
Worlds is a studio wall of hand-drawn plates: each service is a small ink drawing of its own object (a shelf, a door), and every fact is a numbered callout on that drawing. The main gesture is **tap a callout to read its source, freshness, who can see and who can act**. The drawing is the fact card; the pencil line is the provenance.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start: `media-shelf` (Healthy) and `front-door` (Needs a human), fixture ids, names, states and facts verbatim. `#add-service` opens Connect for `notes-sync` (Unavailable): (1) Discover, (2) Credential reference (reference only; a pasted secret is refused with "Worlds stores a credential reference, never the secret"), (3) Safe test (run / running / passed / failed + recovery), (4) Preview `sync.provider-reachable` (unavailable), `sync.local-pending` (3 notes, live), `sync.last-synced` (stale), (5) Choose what to publish (one checkbox per fact), (6) Publish. Step buttons are real `<button>`s. Mock only; no provider call.
One in-memory store keyed by fact id feeds Home, the plate, and Agent facts. A 4s live tick (ON by default) moves freshness ("checked 2s ago" → "12s ago") and now and then marks a fact stale, and all three views update with no reload. `#simulate-change` applies `lock-back-door` to `frontdoor.back-door`: the fact becomes `locked`, Front Door becomes healthy, attention count 0, in all three views at once (the Worlds-side result is a mock review only; nothing is locked). Optimistic: the callout flips at once, then settles with a visible "recorded (mock)" line.

## 3. Navigation between Worlds and a service (phone and desktop).
Persistent shell destinations: Home, Connect, Memory, Settings. Desktop: left rail, plate register in the middle, selected plate and docked app on the right. Phone: bottom tab bar with the same four; a selected plate is a full screen with explicit Back, focus moves to its heading and returns to the invoking row. First screen has Front Door selected, so `#disconnect` is visible without opening anything; it asks "Disconnect Front Door?" with a button labelled "Disconnect" and a Cancel.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The owning app sits in a plain inset frame beside (desktop) or below (phone) the plate, titled with owner and state. Fallback is always visible at its edge: Media Shelf "Open Media app" `app://media`, Front Door "Open Home app" `app://home`, Notes Sync "Open Memory app" `app://memory`. The frame is given no Worlds credential by intent; real enforcement and sandbox flags are UNKNOWN. If embedding fails, the shell, tabs, plate and callouts stay, with a plain explanation and the owner link. Worlds keeps nav, Back, status strip and live toggle around it.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Agent facts is a table over the same records. Columns: fact, source (`local`/`external`/`none`), observed time and freshness word, Agent can see, Human can see, Who can act. `frontdoor.back-door`: agent read, human act ("Locking the door changes the external system and is the person's decision."). `frontdoor.guest-code-used`: agent `none` as a named, focusable, disabled control with the verbatim fixture reason ("Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who."). `frontdoor.unlocked-for`: both read. Acting hands off to Project Home (approvals) or the owning app; never decided in Worlds. Notes Sync keeps provider-unavailable and local-pending-live as separate rows. Retry "Check provider now" is an owner handoff.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
- First run: one blank dotted plate, "A plate waiting for its drawing", a Connect invitation, no invented readings.
- Quiet: Media Shelf, Healthy, a calm drawing. Needs-you: Front Door, "Needs a human" with the fixture reason; marked by a word and a drawn mark, not colour alone.
- Stale / unknown: said in words with age; unknown never blank. Unavailable: Notes Sync, local notes still work.
- Error: a failed safe test shows the reason, a retry button and "nothing was saved".
- Disconnected: plate erased with a gentle redraw-to-blank; says "Front Door is gone from Worlds: its facts, plate and frame. Other services are untouched." Retention UNKNOWN.
- Whimsy, small: a tiny drawn pencil sketches the last connector line when a service connects ("Notes Sync is on the wall"); empty-state sparrow doodle; kind micro-copy. Motion is short (≤300ms), data keeps ticking and the motion is removed under `prefers-reduced-motion`.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Homarr: one integration feeding UI and agent view. Dashy: open the app in place. HA cards: literal state words. MCP ext-apps: contained UI under a host boundary. Different: facts are not a widget grid; each is annotated on an object drawing, and the drawing does not route or map between services.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Skill `hand-drawn-diagrams`: wobbly monoline SVG strokes, numbered leader-line annotations, a stroke-draw on state change (the skill's animated-SVG idea, static fallback). Its Excalidraw output and hosted link are not used; drawings are inline SVG. System `atelier-zero`: Paper `#efe7d2`, Bone `#f7f1de`, Ink `#15140f`, Ink-mute `#5a5448`, Coral `#ed6f5c` as the single accent, Roman section marks (I. Home, II. Connect…), hairline rules, 9.5–38px scale, 18px card radius, transitions ≤0.25s. Adjustments: no external fonts (system serif italic stands in for Playfair, system sans for Inter Tight); coral is non-text only, button labels in Ink (white on coral fails contrast); drop the paper-noise overlay and the 36px side rails (they cost mobile width); no collage photos. Rules applied: anti-ai-slop, state-coverage, accessibility-baseline (4.5:1 text, 3:1 controls/focus, 44px), animation-discipline, no emoji, no invented metrics.

## 9. Signature element, hero thesis, reference lock.
Signature: the annotated plate, a drawn object with numbered callouts tied to fact records. Thesis: "Every fact is pinned to the thing it came from." Reference lock: Monocle's annotated figure plates and Kurzgesagt-free field-guide labelling (mechanics only: numbered keys and leader lines, no palette).

## 10. Risks and UNKNOWNs.
Embed support, enforced isolation, the Connect API, retention after disconnect: UNKNOWN. Wobbly drawings must stay legible and never replace the text state. Leader lines on a narrow screen collapse into numbered rows. Stale events are simulated, not fixture data. Fixture is synthetic; the page stays labelled MOCKUP.

## 11. How it differs from C, and what it keeps.
Keeps: the real shell (Home / Connect / Memory / Settings), fixed owner-labelled services, the app docked beside a shared fact card, honest state words, the fixture's exact permissions, calm structure. Differs: C's bezel and fact table become a hand-drawn plate whose callouts carry the facts; type and paper come from Atelier Zero rather than skeuomorphism; there is a live feed, warmth and motion where C was static; and mobile has its own navigation and reading order.

## 12. Desktop and mobile layouts.
**Desktop, 1440px:** 220px rail (Roman-numbered destinations, MOCKUP label, `#live-toggle` with "Live, checked 2s ago"); a 320px register of services (name, state word, freshness); the main area shows the selected plate with callouts, the docked app frame with its fallback link, and the fact card; `#disconnect` and `#simulate-change` sit in the plate header and `#add-service` in the register header. Agent facts is a destination tab beside the plate.
**Tablet, 768px:** rail collapses to labelled icons; register above, plate and frame stacked.
**Mobile, 390px:** bottom tab bar (four 44px+ tabs); Home is a single column: attention line, then service rows. Selected plate is a full screen: Back, plate (callouts as numbered markers), numbered fact list (tap for a bottom sheet with source, freshness, who sees, who acts), app frame, fallback link, then `#disconnect` and `#simulate-change`. `#live-toggle` sits in the top bar. No sideways scroll; reading order is status, facts, app, actions.
Checker ids: `#add-service`, `#disconnect` (visible on first screen), `#simulate-change`, `#live-toggle`; MOCKUP label always visible.

