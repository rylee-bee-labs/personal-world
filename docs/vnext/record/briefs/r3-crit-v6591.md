# Worlds vNext real round: critique concept v6591's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v6591/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v6591

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
