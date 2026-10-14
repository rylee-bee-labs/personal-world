# Worlds vNext real round: critique concept v8457's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v8457/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v8457

# v8457 — The Counter (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing here is a real reading. A visible "MOCKUP" label sits in the top bar on both layouts.

## 1. Metaphor in one line, and the main gesture
Worlds is a friendly service counter: every service hands its facts across it, and the counter shows plainly where each came from, how fresh it is, who can see it and who can act. Main gesture: **pick a service slip, then read its fact journey** (source → Worlds → agent / you). A small inline-SVG snail courier, **Dot**, carries the facts: slow on purpose, which is the calm.

## 2. Integration model
Start: exactly `media-shelf` (Healthy) and `front-door` (Needs a human). `#add-service` opens Connect for `notes-sync`: (1) discover, (2) credential *reference* (a reference only; a pasted secret is refused with "Worlds stores a credential reference, never the secret"; validates on blur, with dirty and submitted-pending states), (3) safe test (run / running / passed / failed, with recovery), (4) preview of `sync.provider-reachable` (unavailable), `sync.local-pending` (3 notes, live), `sync.last-synced` (stale), (5) choose which facts to publish, one checkbox each, (6) publish. Dot delivers a parcel and a "Connected" stamp lands. All in-memory; no provider call. One store keyed by fact id feeds Home, the service surface and the agent-facts view. `#simulate-change` applies `lock-back-door` (`crossViewExample`): `frontdoor.back-door` → locked, Front Door → Healthy, attention count 0, on all three at once. Worlds locks nothing; it records a mock handoff.

## 3. Navigation between Worlds and a service
Four destinations everywhere: Home, Connect, Memory (holds the aggregate Agent facts view), Settings. Desktop: left rail. Mobile: bottom tab bar; a service opens as a pushed full screen with a Back button and in-page tabs *App / Facts / Agent view*. Focus moves to the new heading and returns to the invoking slip on Back.

## 4. The embedded-app frame
A bordered frame (iframe/ext-app style, drawn) holds the owning app, headed by owner name and state word. The fallback link is always at the frame edge, verbatim from the fixture: "Open Media app" (`app://media`), "Open Home app" (`app://home`), "Open Memory app" (`app://memory`). The frame gets no Worlds credential by intent; real browser enforcement and sandbox flags are UNKNOWN. If embedding fails, the shell, facts and fallback stay, with a plain cause and recovery. Approvals hand off to Project Home; other actions to the owner.

## 5. The agent/human shared-facts model
Each fact row: source (local / external / none), checked time, freshness word, **Agent can** and **Human can** shown separately, plus the reason where one is given. `frontdoor.back-door`: agent read, human act ("Locking the door changes the external system and is the person's decision."). `frontdoor.guest-code-used`: agent none, a named, focusable, disabled control showing the fixture reason verbatim. `sync.provider-reachable` stays unavailable while `sync.local-pending` stays live; they are never merged.

## 6. States
First run (zero services): Dot asleep beside an empty counter, a value sentence and `#add-service`. Quiet: Media Shelf, Healthy. Needs-you: Front Door with its fixture reason. Stale: the live feed can age a service into Stale (labelled "simulated"), and Notes Sync's last sync is Stale. Unknown: said in words, never blank. Error: a failed safe test keeps the typed reference and offers retry, with "last tried Xs ago" and a limit of three tries. Disconnected: `#disconnect` is visible on the first screen for the selected service and opens a confirm dialog with a "Disconnect" button; afterwards Worlds says what is gone ("Front Door's 3 facts and its app frame are removed from Worlds; the app itself is untouched"). Whether history is retained is UNKNOWN. Loading, empty preview and long-copy states each carry recovery copy.

## 7. Borrowed, and what is different
Homarr: one integration feeds the UI and the agent. Dashy: apps open in place. HA cards: literal state words. MCP ext-apps: a contained UI under a host boundary. Different: the fact journey is a list-shaped receipt (the Wise transfer breakdown), not a map, route or topology.

## 8. OpenDesign starting point
Skill `frontend-design` (commit to one direction; real states; self-review at both widths; a memorable quality) and design system `ds-wise`. Used from Wise: off-white canvas, near-black `#0e0f0c`, lime `#9fe870` with `#163300` text for primary pill buttons only (never a large surface), mint `#e2f6d5` for the selected slip, ring shadows `0 0 0 1px rgba(14,15,12,.12)`, 16/30px radii, 8px spacing, heavy display type with tight leading and weight 600 body. Wise Sans and Inter are not available (no external fonts), so I use a heavy system stack, an UNKNOWN for fidelity. Wise's red `#d03238` and yellow `#ffd11a` come with a text label and icon shape, never colour alone. I keep the scale-on-hover behaviour on buttons only, off under reduced motion. Craft rules applied: state-coverage (all five states), accessibility-baseline, animation-discipline, anti-ai-slop (no emoji icons, gradients or invented metrics).

## 9. Signature, hero thesis, reference lock
Signature: the **fact journey receipt** with Dot, which shows source → Worlds → agent / you and settles with a small tick on each change. Thesis: "Every fact says where it came from and who can act on it." Reference lock: Wise's transfer breakdown (mechanics only, not its palette beyond the design system) and Apple Home's accessory states.

## 10. Risks and UNKNOWNs
Embed support and isolation, the Connect API, disconnect history, and font fidelity are UNKNOWN. Whimsy may crowd the calm, so Dot appears only at first run, connect, and state changes. The lime-on-mint contrast is to be measured on the built pairs.

## 11. How it differs from C, and what it keeps
Keeps C's persistent app shell, labelled and owned services, the owning app beside the shared facts, honest states, and the verbatim fixture reasons and fallbacks. Differs: C is a flat, static bezelled cabinet of fixed bays. v8457 drops the cabinet and the bezel and makes a counter with a receipt-style fact journey. It adds warmth through Dot, kind copy and a connect moment, and the live feed makes it reactive: the "checked 2s ago" ticker, simulated staleness and optimistic actions that settle visibly. The skin is Wise (lime, heavy type), not C's skeuomorphism. The feed is on by default, and `#live-toggle` pauses and resumes it, with reduced motion removing animation but not data.

## 12. Desktop and mobile layouts
**Desktop, 1440px:** top bar (MOCKUP label, `#live-toggle` "Live · checked 2s ago", `#simulate-change`, `#add-service`), 232px rail, 360px column of service slips (Front Door is selected on load, and each slip shows state word and freshness), and a detail pane with the service header (state word, **`#disconnect` visible**), the app frame with its fallback link, then the fact journey rows and an "Agent sees" strip under it, so `#simulate-change` visibly changes the slip, the pane and the agent strip together. Tablet at about 768px: the rail collapses to icons and the slips become a top row.
**Mobile, 390px:** a single column with a bottom tab bar and 44px targets. Reading order: the counter's one-line status and live toggle; the selected service card (Front Door first, with state, reason and **`#disconnect` on the first screen**); the other slips; then a floating `#add-service` above the tab bar. Opening a service pushes a screen with Back and tabs (App / Facts / Agent view), and the frame stacks above the facts with the fallback link beneath. Connect is a full-screen stepper with a sticky Back/Next bar. There is no horizontal scroll, and rows wrap, never truncate.

