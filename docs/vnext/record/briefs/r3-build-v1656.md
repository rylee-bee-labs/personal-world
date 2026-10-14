# Worlds vNext real round: revise and build concept v1656

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md` AND `docs/vnext/concept-brief-v2.md` (all of v2 still applies), `docs/vnext/reference/direction-C-DESIGN.md` (the direction Rylee chose), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for concept v1656: skill file `docs/vnext/od-pack/skill-soft-skill.md` and design system `docs/vnext/od-pack/ds-expressive.md`.

1. Write `docs/vnext/round3/v1656/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section.
2. Then build `docs/vnext/round3/v1656/index.html` FROM it. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.
Write only those two files, both before finishing. Do not push.

---
## Your DESIGN.md

# v1656 — The Reading Desk

Status: MOCKUP design brief for concept v1656. Nothing here is approved or built.
Data: `docs/vnext/fixture/fixture.json` only — ids, names, states, values and reasons verbatim, nothing added.

## 1. Metaphor in one line, and the main gesture

Worlds is a reading desk where every service has one stamped card on the same board, and the desk keeps the stamps in step with each other. The main gesture is **read a stamp and see it re-stamp itself**: you pick a service, read what it last said and who may act on it, and you can watch the stamp's ink stay honest while you read.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.

First render holds two connected services: `media-shelf` (Media Shelf, `healthy`) and `front-door` (Front Door, `needs_attention`). `#add-service` starts the six-step Connect lifecycle for `notes-sync` (Notes Sync, `unavailable`): Discover names Notes Sync and its Memory owner; Credential reference accepts a reference only and refuses a pasted secret with "Worlds stores a credential reference, never the secret"; Safe test runs/passes/fails with retry; Preview lists `sync.provider-reachable` (`unavailable`), `sync.local-pending` (3 notes, `live`), `sync.last-synced` (`stale`); Choose what to publish checks each fact id and says what will be shared; Publish creates the Notes Sync card. No provider call happens; this is mock UI.

A service lives as one card in the Home list and as the selected card in the desk view; it owns its own data, its own actions and its own app frame. One in-memory store is keyed by fact id and is the single source for Home, the selected card and Agent facts. `#simulate-change` applies the fixture's `crossViewExample`: `lock-back-door` on `frontdoor.back-door`, so the fact becomes `locked`, Front Door becomes `healthy` and `attentionCount` becomes 0 — Home, the card and Agent facts move in the same state update, with no reload. `#disconnect` is rendered in the selected card's header on the first screen, names the service, and confirms with a button labelled "Disconnect"; after removal the other cards and every shell destination still work and a plain line says what is gone.

## 3. Navigation between Worlds and a service (phone and desktop).

Desktop keeps Home, Connect, Memory, Agent facts and Settings in a persistent left rail; the service list fills the main column; selecting a service reveals the desk — the owning app's frame and the fact card — in the right column. At 390px the rail becomes a bottom bar with the same five destinations, the service list is one column, and selecting a service replaces it with the desk detail plus an explicit Back. Source order follows nav, main, complementary facts. Focus moves to the desk heading on entry and returns to the invoking list button. Everything is keyboard reachable with visible focus and 44px targets.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).

The owning app appears in a flat inset panel with a labelled lip: service name, state word, and the owner line ("Owned by Home"). No drawer travel, press depth or window travel is used as a recognition cue. The fallback link stays visible at the panel edge at all times — Media Shelf "Open Media app" (`app://media`), Front Door "Open Home app" (`app://home`), Nightly Backup "Open Backup settings" (`app://settings/backups`), Garden Camera "Open Home cameras" (`app://home/cameras`), Studio Printer "Open Studio printers" (`app://studio/printers`), Notes Sync "Open Memory app" (`app://memory`). Fixture links use `app://` only. The frame is designed to receive no Worlds credential; whether the browser enforces that boundary and which sandbox flags apply are UNKNOWN. If the frame cannot load, the shell, the fact card and the owner link all stay and the failure is stated in words. Approvals hand off to Project Home; every other action hands off to the owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).

Home, the selected card and Agent facts read the same records. Each fact row states: source (`local` / `external` / `none`), observed time, freshness, **Agent can see**, **Human can see**, and **Who can act** — read and act kept as separate columns. `frontdoor.back-door` is agent `read`, human `act`, with the fixture reason "Locking the door changes the external system and is the person's decision." `frontdoor.guest-code-used` is agent `none` and renders as a named, disabled, focusable control with the exact reason "Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who." `camera.human-detected-today` is agent `none` with its own reason. `sync.provider-reachable` may be `unavailable` while `sync.local-pending` stays `live`; both records survive. `lock-back-door` routes to Project Home and records mock review only; Studio Printer and Notes Sync retries are owner-app handoffs, never approvals.

## 6. States: empty/first run, quiet, needs-you, stale, unknown, disconnected.

First run is reachable from Settings: zero services and zero providers shows a vacant board, the Connect invitation, and the local Worlds/Memory baseline, with no invented reading — `local-weather` (Local Weather, `not_configured`) is the honest zero-provider case and says so in words. Quiet is Media Shelf's `healthy`. Needs-you is Front Door's `needs_attention` plus its `attentionReason`. Stale is Nightly Backup, whose three facts stay `stale` with their given values. Unknown is Garden Camera, never blank and never inferred, carrying its `unknownReason`. Unavailable is Studio Printer and Notes Sync, keeping their local `live` facts alive. Disconnect names what was removed and leaves the shell usable; historical retention is UNKNOWN. Loading, safe-test failure, an empty publish list and long-copy edge cases each get recovery copy.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.

Borrowed: Homarr's one integration feeding several surfaces, Dashy's app opened in place, HA cards' literal state words, MCP ext-apps' contained UI and host-permission boundary. Deliberately different: the desk is a plain reading surface with no map, no route tracing between services, no topology and no drawer or atlas metaphor — the anti-repeat list forbids all of those. What earns the metaphor is that one fact is stamped once and re-stamped identically everywhere it appears, so agreement between Home, the service surface and Agent facts is something you can see rather than a claim.

## 8. OpenDesign starting point (skill + design system) and how you used it.

Starting point: `skill-soft-skill` + `ds-expressive`. The skill is written for marketing pages, so I took only its brief-first discipline, its ban on stock layouts, its ban on banned fonts and thick-stroked icons, and its "make hierarchy obvious, use whitespace before borders" rule; I dropped the parts that fight a product shell — glass, hero gradients, scroll-reveal choreography, magnetic buttons and mandatory 24/32 desktop padding. `ds-expressive` is the binding token set and no value is invented outside it: `#DB2777` primary for at most two accent uses per screen (the primary action and the "needs you" marker), `#2563EB` secondary, `#16A34A` / `#D97706` / `#DC2626` for semantics, `#FFFFFF` surfaces, `#111827` text; IBM Plex Mono for display, body and mono on the 14/16/18/24/32/40 scale; 4/8/12/16/24/32 spacing; 150–250ms transitions on a stable curve. Whimsy is laced in through copy, one 1.6px-stroke inline SVG accent and the stamp's own motion, never through decoration. Applied: status in words and never colour alone, `--accent` capped, contrast measured at 4.5:1 text / 3:1 controls and focus, 390px with no sideways scroll, 44px targets, keyboard reach with visible focus, five required states per surface, `prefers-reduced-motion` honoured with opacity crossfade substituted for transform, no invented metrics, no emoji, and a visible MOCKUP label.

## 9. Signature element, hero thesis, reference lock.

Signature: the stamp — one card per service whose ink is the fact set, and whose live age ("checked 12s ago") is the same counter rendered by Home, the service surface and Agent facts, so a single tick is visibly one event. Hero thesis: "every service keeps its own ink, and Worlds never lets a fact show one age in one place and another elsewhere." Reference lock: Teenage Engineering OP-1's labeled control hierarchy (labels carry meaning, no simulated mechanics) and Things' plain list-plus-detail pair. Motion budget: a 150ms opacity settle on re-stamp and nothing else.

## 10. Risks and UNKNOWNs.

Embed support, browser-enforced isolation, the Connect API shape, the service protocol and disconnect retention are UNKNOWN. IBM Plex Mono is a named system-stack request with a monospace fallback; if it is unavailable the mono character of the stamp weakens, which is a real risk to the signature. A ticking counter can read as noise, so it is pausable by `#live-toggle` and silent under reduced motion. Fixture data is synthetic and stays labeled MOCKUP. Contrast on implemented pairs must be measured before review, not assumed.

## 11. How it differs from C, and what it keeps.

C is a cabinet of fixed bays where the selected bay docks a static bezel beside a shared fact card. v1656 keeps C's real application shell (Home / Connect / Memory / Settings), its six-step Connect lifecycle with credential-reference-only, its docked owning-app frame with a permanently visible `app://` fallback, its literal state words, its disabled-with-a-reason permission controls, its honest distinction between provider failure and local-data health, and its governed handoff to Project Home. It changes the frame and the rhythm: instead of a cabinet whose bays are read in fixed order, the desk is a live reading surface whose signature is **time** — one fact, one ink, one age, re-stamped identically in all three views, with a visible pause control and a quiet drift toward stale. Where C's signature is a static inset bezel, this one's is a stamp that visibly changes while you watch. C has no live feed, no settings-level first run and no mobile-native navigation pattern; v1656 designs the bottom bar, the sheet-style detail and the pause/resume control on purpose rather than deriving them from the desktop layout. Nothing in v1656 copies C's register, its bay order, or its bezel.

## 12. Desktop and mobile layouts.

Desktop (1440px): a 200px persistent left rail (Home, Connect, Memory, Agent facts, Settings) with the MOCKUP label and the live-feed control beneath it; a 360px service list column showing every connected service as a stamp card with its state word and age; and a flexible desk area where the owning app's inset panel sits above the fact card, with `#add-service` in the list header and `#disconnect` in the selected card header. Three columns, one reading order: rail → list → desk.

Mobile (390px): no left rail. A sticky top bar holds the current destination and the live-feed pause/resume; a fixed bottom bar holds Home, Connect, Memory, Agent facts, Settings with 44px targets and the selected destination marked by icon plus label, never colour alone. The service list is a single column of full-width stamp cards, each at least 44px tall, with the state word above the age. Selecting a card pushes the desk detail as its own screen: heading, state word, `#disconnect`, the owning app panel, then the facts. The Connect lifecycle runs as a stepped sheet with its own Back and a sticky action row, and the confirmation for disconnect is a full-screen sheet with the confirm button labelled "Disconnect". Reading order is top bar → content → bottom bar, and the bottom bar never overlaps content. Tablet around 768px is a bonus: rail collapses to icons, list and desk sit side by side.

Live feed and reactivity: `#live-toggle` pauses and resumes the tick and is visible in the desktop rail and the mobile top bar; the feed is on by default and reduced motion silences animation but not data. `#simulate-change` applies `lock-back-door` and updates Home, the Front Door card and Agent facts at once. Freshness ages from "checked 2s ago" to longer readings on every tick, and a service may move to a stale reading over time with a visible state-word change and a 150ms opacity settle; under reduced motion the settle becomes an instant swap with the state word as the only signal. Optimistic actions give instant feedback and settle visibly.

---
## CRITIQUE.md (another engine)

# Critique of v1656 "The Reading Desk" DESIGN.md

Five gaps, most important first. Reviewed against concept-brief-v3 (and v2), direction-C-DESIGN.md and fixture.json.

## 1. The live feed is a clock, not a reactive model, and parts of it are dishonest
**Missing/wrong.** Brief v3 item 4 asks for a feed that changes freshness, occasionally moves a service to stale, updates every view, gives optimistic feedback that settles visibly, and has calm transitions. §12 gives each of these one clause:
- Ages only count up. Nothing ever re-checks and resets them.
- "A service may move to stale over time" names no service, no trigger and no threshold. The fixture defines none, so any threshold is an invented rule. This breaks the v2 floor "nothing presented as a real reading".
- "Optimistic actions give instant feedback and settle visibly" says nothing about which actions, which pending state, or what settles into what.
- §5 says `lock-back-door` "routes to Project Home and records mock review only". §2 says the fact becomes `locked` and Front Door `healthy` at once. If Worlds does not decide, then showing `locked` immediately is exactly the "decision made in Worlds" that requirement 5 forbids. A "requested / awaiting Project Home" state is missing.
- The "same counter" thesis (§9) conflicts with the fixture, where ages differ per service (28s, 24s, 106s, 266s...). It also gives no aria-live policy for a counter that ticks every few seconds.
- The motion budget of "a 150ms opacity settle and nothing else" leaves no room for the visible state transitions the brief asks for.

**Fix.**
- Name a scripted event list built only from fixture facts, for example Studio Printer re-checks and resets its age, and Media Shelf's age drifts until it is labelled `stale` at a stated demo threshold marked "demo rule, not a fixture value".
- Add an explicit `pending-handoff` state for `lock-back-door`, shown as "Sent to Project Home - waiting". Flip the fact only as a labelled mock outcome.
- Specify per-service ages derived from `checkedAt`.
- Make the tick visual only (`aria-live=off`), with state changes announced once, politely.
- Define the transition set: state word swap, row settle, and the Connect "connected" moment.

## 2. It is not yet a daily-use application: most states are unreachable and most destinations are empty
**Missing/wrong.** Brief v3 item 1 asks for real navigation, a settings screen, a first-run state and an error state.
- §2 and §6 start with only Media Shelf and Front Door, plus Notes Sync via Connect. Nightly Backup (stale), Garden Camera (unknown), Studio Printer (unavailable) and Local Weather (not_configured) are cited as the stale, unknown and unavailable examples, but nothing says how they ever appear. Three of the six required states are therefore unreachable in the prototype.
- Home, Memory, Agent facts and Settings are listed in the nav, but only Settings gets one line (it is the entry to first run). Nothing says what Home shows. It is unclear whether Home is the service list or a summary with `attentionCount`.
- No Settings content is defined. The live-feed control, the provider list and the Local Weather "choose a provider" path are all missing.
- The error state is "recovery copy" for safe-test failure with no wording, no retry path and no runtime drop-off.

**Fix.**
- Add a Settings screen with the live-feed on/off and interval, a Providers list that includes Local Weather as `not_configured` with its `emptyReason`, a "Start over (first run)" action, and a "Reset demo" action.
- Pin Home to a one-line summary plus an attention list.
- Give Memory and Agent facts at least their row structures.
- Add an "Add a service" demo picker, or a scripted drop-off, that brings Nightly Backup, Garden Camera and Studio Printer into the list, so every state has a path.
- Write the safe-test failure copy and its retry/back actions.

## 3. Mobile and the checker contract: `#disconnect` and `#simulate-change` fail on a phone, and the layout is inconsistent
**Missing/wrong.**
- v3 requires `#disconnect` to be visible on the first screen for the currently selected service. At 390px (§12) the first screen is the service list. `#disconnect` appears only after a card is pushed to its own screen, and no service is selected on the list.
- `#simulate-change` is never placed at either width.
- §11 says "sheet-style detail". §12 says a pushed screen plus a full-screen sheet for the disconnect confirmation. These are two patterns for one thing.
- A sticky top bar, a fixed bottom bar and a sticky Connect action row stack up at 390px with no height budget. Combined with the 44px targets, this risks content being squeezed or covered.
- The desktop layout has no stated behaviour between 1440 and 768. The "bonus" collapses the rail to icons at 768, but the three columns total 560px of chrome before the desk.

**Fix.**
- At 390px, preselect the first service (Front Door, the needs-you one). Show a compact selected-service header with `#disconnect` above the list, or show the detail directly on first load and make the list the Back target.
- Place `#simulate-change` in the Front Door card on both layouts, and say so.
- Choose one detail pattern (pushed screen) and use a sheet only for confirmations.
- State the chrome heights (for example 56px top, 64px bottom) and require `padding-bottom` for the content so nothing is covered.

## 4. Whimsy and warmth are asserted, not designed
**Missing/wrong.** v3 item 2 asks for kind micro-copy, a small inline-SVG illustration or character, a satisfying "connected" moment and a gentle motion on state change.
- §8 spends "whimsy" on "copy, one 1.6px-stroke inline SVG accent and the stamp's own motion, never through decoration". No copy is written, the accent has no subject, and no connected moment exists beyond the Publish step.
- The stamp metaphor never becomes a visible thing: no stamp shape, no ink behaviour, only a 150ms opacity fade.
- The all-IBM-Plex-Mono type system, a sharp `#111827` on white palette and a motion budget of "nothing else" read as cold and clinical.
- Pink `#DB2777` doubles as the primary action and the "needs you" marker, so one hue carries two meanings.

**Fix.**
- Name the SVG (for example a small ink-pad-and-stamp pictogram that appears in the empty board and in the "connected" moment).
- Write 6-8 specific lines: first run, connect success ("Notes Sync is on the desk"), disconnect ("Notes Sync has left the desk; your notes stay in Memory"), a quiet-day Home line and a stale line.
- Add one calm "stamp lands" transform (a small scale-and-settle) on publish and on re-stamp, replaced by an opacity swap under reduced motion.
- Reserve pink for the primary action and use `#D97706` plus the words "Needs a human" for needs-you.

## 5. Difference from C is thin, and the comparison to C is partly wrong
**Missing/wrong.**
- §11 lists everything it keeps (shell, six steps, frame, fallbacks, state words, disabled-with-reason controls, handoff) and names one change: time. The metaphor is a rename of "bay" to "card". The OP-1 reference lock is identical to C's. A rail, list and detail reading order is C's layout.
- It also claims "C has no mobile-native navigation pattern". C §3 specifies a 390px one-column register, a replacing detail screen, persistent shell destinations and an explicit Back. v1656's own §3 is the same, with a bottom bar added.
- §8 uses the soft skill almost only subtractively (it lists what was dropped), so the skill is barely visible in the design. `ds-expressive` is applied as a token list, with no sign of what makes it "expressive".
- The anti-repeat list is respected, but only because nothing new is added. A stamp metaphor with no visible stamp does not give it its own look.

**Fix.**
- Correct §11 to say C already has a basic mobile pattern and that v1656's addition is a bottom bar.
- Make the stamp a real, distinctive visual unit (ink block, state word as the stamped mark, age as a printed edge) and a different reference lock (for example Things' list-detail plus a postal-franking mark, taking the mechanics only).
- Name at least two things from `ds-expressive` and `skill-soft-skill` that visibly shape the output, such as the pink accent budget, the type scale and card radius, beyond "I took the discipline".

