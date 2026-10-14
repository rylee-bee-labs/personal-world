# Worlds vNext real round: revise and build concept v6591

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md` AND `docs/vnext/concept-brief-v2.md` (all of v2 still applies), `docs/vnext/reference/direction-C-DESIGN.md` (the direction Rylee chose), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for concept v6591: skill file `docs/vnext/od-pack/skill-stitch-skill.md` and design system `docs/vnext/od-pack/ds-retro.md`.

1. Write `docs/vnext/round3/v6591/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section.
2. Then build `docs/vnext/round3/v6591/index.html` FROM it. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.
Write only those two files, both before finishing. Do not push.

---
## Your DESIGN.md

# v6591 — The Board (Platform One)

MOCKUP. A design prototype, not a product. Every value comes from
[`fixture.json`](../../fixture/fixture.json); nothing here is a real reading.

Inputs: [concept-brief-v3.md](../../concept-brief-v3.md), [concept-brief-v2.md](../../concept-brief-v2.md),
[design-research.md](../../design-research.md). Starting direction: [direction-C-DESIGN.md](../../reference/direction-C-DESIGN.md).

## 1. Metaphor in one line, and the main gesture

Worlds is a small station's departure board: every service you own is a **service**
(the train kind too), and the board says — in a word, with a time — what it last
reported and who may act on it. The main gesture is **glance at the board in the
morning, see what needs a human, and step up to that service's counter** to use its
own app. The double meaning is the warmth; the structure underneath is a calm,
literal status board.

## 2. Integration model: how a service plugs in, where it lives, how facts flow

Start state is exactly two connected services: `media-shelf` (Healthy) and
`front-door` (Needs a human). They are not equal-bayed owners in a fixed register —
they are rows in one live board **ordered by urgency, then freshness**: needs-you
first, then stale/unavailable/unknown, then healthy. Order changes only when a
state changes, never on a tick.

`#add-service` opens Connect as a six-step stepper — Discover → Credential
reference → Safe test → Preview → Choose what to publish → Publish. Discover lists
the fixture's not-yet-connected services with their verbatim fixture states
(`nightly-backup` Stale, `garden-camera` Unknown, `studio-printer` Unavailable,
`notes-sync` Unavailable, `local-weather` Not configured). **Default path is
`studio-printer`**, because its real error (`job rejected: no media loaded`) and its
human-only `Retry last job` exercise the states a daily app actually hits;
`local-weather` is selectable to show the honest zero-provider empty. Credential
step accepts a *reference* only and rejects a secret with "Worlds stores a
credential reference, never the secret." Test step shows run → running → passed/failed.
Preview lists the fixture fact IDs and states verbatim (printer: `printer.status`
`unavailable`, `printer.queue` 1 `live`, `printer.last-error-code` `E-MEDIA-02`
`live`). Publish adds the service with only the chosen facts.

One in-memory store keyed by fact ID feeds every view. `#simulate-change` applies the
fixture's `crossViewExample` exactly (`lock-back-door`): in a single state update
`frontdoor.back-door` becomes `locked`, Front Door becomes `healthy`, the board's
needs-you count goes to 0, the service surface and the agent-facts view all re-render
together. `#disconnect` confirms by name, removes the service and its counter, and
says plainly what is gone.

## 3. Navigation between Worlds and a service

Desktop: persistent left rail (Today, Connect, Memory, Settings), the board in the
main column, and the selected service's **counter** in a right pane. Mobile: a
bottom tab bar (same four destinations, same order), the board as one scrolling
column, and a service is a **full-screen page** with an explicit Back — not a squeezed
pane. Connect is a full-screen stepper on mobile and a centred column on desktop.
Focus moves to the service page heading on entry and returns to the invoking row on
Back. DOM order is nav → main → complementary agent-facts, so it never reorders under
reduced motion.

## 4. The embedded-app frame

The counter is the main surface of a service view, not a decorative inset: a flat
bounded screen with a printed header naming the owner app, its state word, and the
fixture fallback link (`Open Media app` / `Open Home app` / `Open Studio printers`,
`app://` only) pinned at the frame edge at all times. Worlds' navigation, the live
toggle and the service's own controls keep working around it. If embedding fails, the
frame is replaced by what happened, why, and the owner link — the rest of the service
view survives. Worlds passes no credential into the frame by intent; whether the
browser enforces that, the sandbox flags, and app messaging are UNKNOWN. Decisions
hand off: locking the back door opens a confirmation that routes to the owning Home
app and records mock review only — Worlds never locks anything itself.

## 5. The agent/human shared-facts model

Every fact row, on the counter and in Agent facts, carries source, checked time,
freshness word, and permissions split two ways: **Agent can see** and **Human can
see**, then **Who can act**. `frontdoor.back-door`: agent `read`, human `act`, with
the fixture reason verbatim. `frontdoor.guest-code-used`: agent `none` renders as a
named, disabled, focusable control with the fixture reason verbatim — never hidden.
`frontdoor.unlocked-for`: both `read`. If Garden Camera is connected,
`camera.human-detected-today` gets the same treatment with its own reason.

## 6. States

**First run / zero services** — empty board, the local Worlds baseline, and a Connect
invitation; no invented readings. **Zero providers** — `local-weather` is
`not_configured` with no facts and says so in the fixture's own words. **Quiet** —
`media-shelf` Healthy. **Needs you** — `front-door` with its fixture
`attentionReason`. **Stale** — `nightly-backup`, and `sync.last-synced`-style facts.
**Unavailable** — provider failure is never collapsed into local-data failure.
**Unknown** — `garden-camera`, with its `unknownReason`, never a blank. **Disconnected** —
names the service, keeps the rest of the board working. Loading, safe-test failure,
empty preview and long-copy each get recovery copy. Status is never colour-only.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps

One integration feeding many surfaces (Homarr), per-item checked-at status (Dashy),
`unavailable`/`unknown` as first-class rendered states (HA cards), sandboxed contained
UI with a plain fallback (MCP ext-apps). Deliberately different: the board is
time-and-urgency ordered and ticking, so the same fact is read at a different place
on the board as it ages; and a permission that is `none` is shown disabled with its
reason rather than omitted.

## 8. OpenDesign starting point

Starting point: [`skill-stitch-skill.md`](../../od-pack/skill-stitch-skill.md) +
[`ds-retro.md`](../../od-pack/ds-retro.md). From the skill: semantic named tokens with
exact hex and roles, an explicit banned list, and the "80% proven / 20% distinctive"
split — the 20% is the flap and the lamp. From ds-retro: the 4/8/12/16/24/32 spacing,
the literal-label voice, and 150–250 ms transitions with `#3B82F6` as the interaction
signal. Deliberately **not** taken: the skill's perpetual micro-loops on every
component (contradicts [animation-discipline.md](../../od-pack/animation-discipline.md)
and the ambient-motion pause rule) and ds-retro's `#8B5CF6` secondary (banned indigo
under [anti-ai-slop.md](../../od-pack/anti-ai-slop.md) sin 1). Macondo appears only as
one small margin-note voice — never a heading, label, button or value; JetBrains Mono
carries every number and freshness time; body is a declared grotesque stack, no
webfonts. Accent text uses a darkened in-family step per
[color.md](../../od-pack/color.md); only two visible accent uses per screen. Guide
rules applied: single job, hero as thesis, ONE signature element, reference lock of two.

## 9. Signature element, hero thesis, reference lock

**Signature: the flap.** One two-cell split-flap strip carries the selected service's
state word and its freshness time, and rolls to the new value when the feed or an
action changes it — 180 ms, ds-retro easing. Under reduced motion the same cell
re-renders the new value with a brief tint and a polite `role="status"` announcement.
One warm character accent: a small inline-SVG lamp, lit while the feed runs, dimmed
with the written word "Paused" when it does not.

Hero thesis: **"One board, one clock, nothing guessed."**

Reference lock (mechanics, not palettes): Network Rail **Solari split-flap** boards —
a value that changes is *shown changing* — and Things 3's **Today** sectioning —
severity first, then order.

## 10. Risks and UNKNOWNs

A reordering board can move a row under the pointer: mitigated by ordering only on
state change and announcing politely. Flap motion may cost legibility for long words:
cells hold one word, and `Needs a human` is shortened only where the full fixture
label stays in the accessible name. `app://` links will not resolve in a browser;
actual embed support, sandbox enforcement, the Connect API and disconnect history are
UNKNOWN. Fixture data is synthetic and stays labelled MOCKUP. Contrast must be
measured on the implemented pairs before review.

## 11. How it differs from C, and what it keeps

C is a **static cabinet of fixed, equal-order bays**; this is a **live board ordered
by what needs a human**, where a row's position and freshness change under the tick.
C docks the embedded app *beside* the fact card; here the app **is** the main surface
of the service view, with the flap strip above it and the permissions "ticket" below.
C is motionless with a skeuomorphic bezel; this is flat retro print with exactly one
signature motion. C's third service is `notes-sync`; the default path here is
`studio-printer` (real error, human-only retry) with `local-weather` for the honest
empty. C shares a shell rail on both sizes; mobile here gets a bottom tab bar and
full-screen pages. **Kept from C, because they were right:** the four shell
destinations, credential-reference-only, the permanent owner fallback at the frame
edge, the verbatim fixture state vocabulary and permission reasons, the disabled-with-
reason rule, the fixture `crossViewExample` as the cross-view demonstration, and the
rule that governed actions hand off to the owning system.

## 12. Desktop and mobile layouts

**Desktop, 1440px.** 240px rail; main column max 720px holding the flap header, the
live toggle, and the board; a 400px right pane holds the counter for the selected
service with agent-facts beneath it. 8px grid, 24px gutters, no horizontal scroll.

**Mobile, 390px.** No rail. A 56px top strip holds the wordmark, the live pill
(`#live-toggle`) and the MOCKUP label; content is one column, 16px gutters; a 64px
bottom tab bar carries the four destinations at 44px minimum targets. Reading order is:
needs-you line → board rows (state word, freshness, one fact line, `Disconnect` if
selected) → the selected service's full-screen page (flap strip, counter, fallback
link, agent-facts) → Connect as its own stepper screen. `#disconnect` for the
currently selected service is on the **first screen**, visible without opening
anything. No sideways scroll at 390px; long labels wrap, never truncate into
meaninglessness.

**Tablet, ~768px (bonus).** Rail becomes a horizontal top strip; board and counter sit
side by side at a 1.2fr / 1fr split.

## Checker contract

`#add-service`, `#disconnect` (visible on the first screen, confirmed by a button
labelled "Disconnect"), `#simulate-change`, `#live-toggle` (feed ON by default;
reduced motion stops animation, never the data). Plus a visible **MOCKUP** label.

---
## CRITIQUE.md (another engine)

# Critique of v6591 DESIGN.md ("The Board") — 5 gaps, most important first

Reviewer: a different author. Graded strictly against `concept-brief-v3.md` (v3) and `concept-brief-v2.md` (v2).

## 1. Reactivity is the pitch, but the feed is unspecified and the rules contradict each other
**Missing/wrong.** v3 item 4 asks for a gentle tick, an occasional move to stale, every view updating without reload, optimistic feedback that "settles visibly", and a pause control. The DESIGN.md never states the tick interval, what the tick changes, or which service goes stale and when. It also contradicts itself on ordering. §2 says order "changes only when a state changes, never on a tick", but §2 also orders by "urgency, then freshness", and §7 and §11 say "a row's position and freshness change under the tick". Both cannot hold. The reference lock is "a value that changes is *shown changing*", yet the flap (§9) exists on only one strip, for the selected service. Every other row on the board changes silently. Nothing specifies optimistic feedback for `Retry last job` or for the lock handoff. §9 covers only the paused lamp, not what Worlds does while paused.
**Why it matters.** Reactivity is the one thing the owner explicitly asked for, and the concept's metaphor depends on it. As written, a builder has to invent the feed, and the rows can jump under the pointer, which §10 claims to prevent.
**Fix.** Add a "Live feed" section. State the tick (for example 3 s, freshness text only). State one scripted event (for example `media-shelf` goes Stale at about 40 s, as a visible reorder with a settle animation and a polite announcement). Drop "freshness" from the sort key, so sort is state severity then fixed fixture order. Give each row a small per-row change cue. Specify optimistic states for `Retry last job` (pending → settled or failed) and for the lock handoff. Define the paused behaviour: ages freeze, a "Paused" banner appears, and `#simulate-change` still works.

## 2. The two layouts contradict the brief's checker contract and the "app is the main surface" claim
**Missing/wrong.** §11 claims "the app **is** the main surface of the service view", differing from C, which docks the app beside the fact card. §12 puts the counter in a 400px right pane, beside a 720px board, with agent-facts beneath. That is C's docking arrangement, and the embedded app is squeezed to 400px. The DESIGN.md never says what is drawn inside the frame (v2 req. 3). On mobile, a service is a "full-screen page" (§3), but `#disconnect` must be "visible without opening anything" on the first screen (§12; v3 checker contract). Those can't both be true, and the DESIGN.md never says which service is selected by default. It lists "`Disconnect` if selected" on every board row, which would repeat the id. `#add-service`, `#simulate-change` and `#live-toggle` are never placed on either layout. The agent-facts view is only a complementary region of one service, so on mobile `#simulate-change` cannot visibly update Today, the service page and agent-facts at once (v2 req. 4; v3 checker).
**Why it matters.** The checker script and requirements 3, 4 and 6 will fail or be faked. The difference-from-C paragraph, which v3 requires, is partly false.
**Fix.** Pick one story. Either make the counter truly primary (desktop: board 360px, service view wide, with the agent-facts strip below) or drop the claim. Name a default selected service (Front Door) and put one `#disconnect` in the first-screen header of the selected service (a compact "selected" card above the board on mobile). Specify the frame's contents (an app header, two or three of the service's facts as the owner app would draw them, the fallback link). Place `#add-service` (rail and a board footer button), `#simulate-change` (a "Try a change" control in the needs-you strip) and `#live-toggle` (the mobile top strip and the desktop header). Add an Agent facts destination or a mobile sheet so propagation is visible in one place.

## 3. "Real daily-use app" is claimed for the shell but Settings, Memory, first-run and error flows are only named
**Missing/wrong.** v3 item 1 requires "real navigation, a settings screen, a first-run / empty state, an error state". The DESIGN.md lists four destinations (Today, Connect, Memory, Settings), but Memory and Settings have no content at all. Settings has no controls (live feed default, reduced-motion override, credential references, disconnect history). First run (§6) is one sentence. The Connect failure path (credential rejected, test failed) has no screen, no copy and no recovery. The same goes for a service dropping off mid-session. "Loading, safe-test failure, empty preview and long-copy each get recovery copy" is a promise, not a design. Also, the renamed "Today" is Home everywhere else (C, v2, v3), with no mapping noted.
**Why it matters.** This is the difference between "a concept on a page" (the v2 complaint) and an app the owner would open daily. Two of four nav destinations are empty.
**Fix.** Add one short paragraph each for Settings (feed on/off and interval, credential references list with revoke, reduced-motion note, MOCKUP/about), Memory (what it shows from the fixture, or drop the tab to three), first-run (the actual copy and the one illustration, with the Connect CTA), and the Connect error and drop-off states (the exact copy for `job rejected: no media loaded`, the recovery buttons). Say "Today = Home".

## 4. Whimsy is thin, and the board is close to a ledger the anti-repeat list already bans
**Missing/wrong.** v3 item 2 wants "kind micro-copy, a tiny illustration or character accent in inline SVG, a gentle motion on a state change, a satisfying 'connected' moment". The DESIGN.md offers a flap, a lamp, and a Macondo "margin note" voice with no copy written. No connected moment is specified at all (Publish is just "adds the service"). The thesis "One board, one clock, nothing guessed" and the "calm, literal" structure read cold, not warm. The board is also an urgency-sorted list of status rows with state words and times. That is structurally the round-2 "brutalist service ledger", "editorial ledger of facts", "newsroom night wire" and "daily edition", and the round-1 "now-line". The departure-board look is the only separator, and the DESIGN.md never argues the difference. "Static cabinet" for C is partly a straw man, since C's own §6 allows reduced motion.
**Why it matters.** Rylee's selection of C was for warmth plus real use. Without specified moments, the builder will ship a status table with a lamp, and the anti-repeat rule is breached by structure.
**Fix.** Write the moments: 3 lines of kind micro-copy (empty board, all-clear, paused), the connected moment (the new row's flap rolls from a blank to its state, with copy such as "Studio Printer is on the board."), and the lamp's SVG behaviour (lit, dimmed, "asleep" for paused). Add one sentence on why this isn't a ledger: the rows are *services at a counter*, with the flap as the only table-like device. Say what's deliberately dropped from the ledger lineage (no columns, no rule-lines).

## 5. OpenDesign use is asserted, not pinned, and contains contradictions
**Missing/wrong.** §8 says the skill gives "semantic named tokens with exact hex and roles", but the DESIGN.md pins none: no palette, no state colours, no type scale, no radius, no easing value ("ds-retro easing" is not a value). v2's guide rule is "decision coverage… pin decisions before code". It bans webfonts ("no webfonts") yet names Macondo and JetBrains Mono, and gives no fallback stack, which violates the v2 stack rule (no external fonts). `#3B82F6` is named as the interaction signal. On white it is roughly 3.7:1, which fails 4.5:1 for text, and the document says contrast "must be measured" later (§10). "Only two accent uses per screen" conflicts with `#3B82F6` as the interaction signal on every control and focus ring. The two skill pieces that are rejected (perpetual loops, `#8B5CF6`) are good calls, but little of the system remains besides spacing.
**Why it matters.** Without pinned values two builders produce two products, and the guide is what keeps the output from default slop.
**Fix.** Add a token table: surface, ink, muted, one accent and its darkened text step (state the ratios), four state colours each paired with a word, the type scale, the radius, durations and the exact easing curve. Resolve fonts: either system stacks only (say which monospace stack carries numbers) or drop the two names. Define "accent use" (decorative fills) separately from focus and interaction indicators.

