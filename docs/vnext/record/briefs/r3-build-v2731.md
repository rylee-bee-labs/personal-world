# Worlds vNext real round: revise and build concept v2731

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md` AND `docs/vnext/concept-brief-v2.md` (all of v2 still applies), `docs/vnext/reference/direction-C-DESIGN.md` (the direction Rylee chose), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for concept v2731: skill file `docs/vnext/od-pack/skill-impeccable-design-polish.md` and design system `docs/vnext/od-pack/ds-warm-editorial.md`.

1. Write `docs/vnext/round3/v2731/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section.
2. Then build `docs/vnext/round3/v2731/index.html` FROM it. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.
Write only those two files, both before finishing. Do not push.

---
## Your DESIGN.md

# v2731 — Worktable

Status: MOCKUP design doc for round-3 concept **v2731 "Worktable"**; nothing here is approved, real, or a
reading. Data is the shared synthetic [fixture](../../fixture/fixture.json); the decisions in
[concept-brief-v3.md](../../concept-brief-v3.md) and [concept-brief-v2.md](../../concept-brief-v2.md) hold
unchanged. Direction chosen: [direction-C-DESIGN.md](../../reference/direction-C-DESIGN.md).

## 1. Metaphor in one line, and the main gesture
Worlds is a worktable under a labelled wall: every service hangs on its own named tag, and one fact sits on
the table as a single card wherever it is read. The main gesture is **take a tag down — its owning app comes
to the table beside the fact card — and hang it back**. Nothing unfolds, routes or draws a cable.

## 2. Integration model: how a service plugs in, where it lives, how its facts flow
First render: `front-door` (Needs a human) and `media-shelf` (Healthy) on the wall, Front Door on the table.
`#add-service` runs the third service through Connect's six steps — **discover → credential reference → safe
test → preview → publish** — as real `<button>`s with plain labels; the credential step takes a *reference*
only, rejecting a pasted secret with "Worlds stores a reference to your credential, never the secret"; test
shows run / running / passed / failed with recovery; preview lists `printer.status` (Unavailable),
`printer.queue` (Live, 1 job), `printer.last-error-code` (Live, E-MEDIA-02), one checkbox per fact, and a
sentence of what will be shared; publish puts **Studio Printer** on the wall. Discover also lists
`nightly-backup`, `garden-camera`, `notes-sync` and `local-weather` with their fixture states, so Stale,
Unknown and Not configured are reachable, never invented. Home, the table and Agent facts read one in-memory
store keyed by fact id. `#simulate-change` applies the fixture's `crossViewExample` (`lock-back-door` on
`frontdoor.back-door`) in one update — door `locked`, Front Door Healthy, attention count 0 — changing Home,
the Front Door surface and the Agent facts view at once, without a reload.

## 3. Navigation between Worlds and a service (phone and desktop)
The shell destinations are persistent: **Home, Connect, Memory, Settings**. Desktop keeps rail, wall and
table on one screen; a service is never a separate route. Phone uses a bottom bar and opens a service as a
sheet over Home, Escape or Back returning. Focus goes to the table heading on open and back to the tag on
close; one tab stop per tag, `aria-current` on the selected one.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it)
The owning app runs in a sharply bounded inset frame on the table; owner, state word and fallback stay
outside it and stay visible — "Open Media app" (`app://media`), "Open Home app" (`app://home`), "Open Studio
printers" (`app://studio/printers`). Worlds passes no credential into the frame. If embedding fails the shell
keeps the card and the tag, says the frame could not be shown, and keeps the owner link. Approvals hand off
to Project Home, other actions to the owning app. Sandbox flags, messaging and provider protocol: UNKNOWN.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts)
Each card shows source, checked-at (recomputed from `checkedAt`, never trusted from `ageSeconds`) and a
freshness word, then **Agent can see** / **You can see** / **Who can act**. `frontdoor.back-door`: agent read,
human act, reason "Locking the door changes the external system and is the person's decision."
`frontdoor.guest-code-used`: agent `none` as a named, disabled, still-focusable control with its verbatim
reason, "Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not
who." `frontdoor.unlocked-for`: both read. `camera.human-detected-today`: the same while its source is
unknown. A `none` is never hidden or blank.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected
First run: zero services shows an empty table — one hand-drawn wren, a headline, one CTA (`#add-service`);
zero providers shows Local Weather as "Not configured — nothing is guessed". Quiet: Media Shelf, Healthy.
Needs you: Front Door's exact `needs_attention` state and reason. Stale: Nightly Backup and `sync.last-synced`,
word "Stale" with "checked 6h ago". Unknown: Garden Camera, "no source has answered since" — never
inferred, never blank. Disconnected: a plain banner naming what is gone and what stopped updating. Connect
also carries loading (skeleton, 15 s notice), error (cause, recovery, input preserved) and an honest empty
preview. Status is always a word plus a non-colour marker.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different
**Borrowed:** Homarr's one integration feeding many surfaces; Dashy's in-place app opening; Home Assistant
cards' explicit entity states with actions declared per object; MCP ext-apps' sandboxed UI and
host-mediated permission. **Different:** no dashboard tile grid, no configuration surface, no topology, no
map; the register is a fixed-order ownership wall, the only composed surface is the fact card, and the
embedded tool asks the shell for an action rather than calling a service directly.

## 8. OpenDesign starting point (skill + design system) and how I used it; guide rules applied
`skill-impeccable-design-polish` shapes the *finish* — audit → critique → five gaps → polish → harden →
animate — with its AI-tell removals applied literally (no purple-blue glow, no generic feature row, no
oversized rounded cards everywhere, no marketing adjectives, no decoration that does not aid comprehension).
`ds-warm-editorial` supplies every value: paper `#FAF7F2`, ink `#1C1A17`, terracotta `#C0512F`, forest
`#2F5B4F`, muted `#8A817A`; serif display over a system sans; 12/14/16/20/28/40; 1200 px, 12 columns; 12 px
buttons, 16 px cards; underline inputs; two elevation levels. Applied: **anti-ai-slop** (serif headings, no
emoji, no gradients, no invented numbers, no rounded card with a coloured left border, fixture copy only),
**typography** (three weights, ALL CAPS at 0.07em, 40 px+ at −0.02em), **animation-discipline** (150 ms
confirmations, ≤300 ms sheets, one-shot wren, opacity-only under reduced motion), **laws-of-ux** (≤5 options
per decision, von Restorff on the selected tag with a word, peak-end on publish), **state-coverage**,
**accessibility-baseline** (WCAG 2.2 AA, native controls, `:focus-visible`, live region present before
content). **Measured conflict, floor wins:** terracotta on paper is ≈4.41:1 and `--muted` ≈3.6:1, under 4.5:1
for small text, so small text uses `--fg`, terracotta only on `--surface` (≈4.7:1) or as a fill/edge, with a
warning comment where the artifact departs from the system's usage note.

## 9. Signature element, hero thesis, reference lock
**Signature: the fact card** — byte-identical on Home, the service surface and Agent facts, with source,
checked-at dateline, freshness word and two named permission rows. **Hero thesis: "Every service keeps its
own name on the wall; every fact is one card with a source." Reference lock, mechanics only:** Home Assistant
entity cards (states first-class, actions per object) and Field Notes' label grammar (owner, place, date).

## 10. Risks and UNKNOWNs
UNKNOWN: embed support, browser-enforced isolation, provider protocol, Connect API details, disconnect
retention. The live tick can become noise (one "checked" line, a visible pause, no motion under reduced
motion); the wall can be mistaken for navigation (each tag names an owner and a state word, not a
destination); contrast must be re-measured on built pixels; scope can drift into a dashboard — the one job is
"which of my services needs me, and what does Worlds actually know".

## 11. How it differs from C, and what it keeps
C is a cabinet of enclosed bays: register left, the selected bay's inset screen and fact card right, a static
bezel as its signature. Worktable keeps the shell but opens it: the register is a wall of named tags *below*
the composition, the table *above* holds the owner's app and the card, and the hero is the card, not the
frame; the reference lock is Home Assistant cards and Field Notes, not OP-1 and Apple Home. **It keeps**
persistent Home / Connect / Memory / Settings; services as clearly owned, labelled places in a fixed reading
order; the owning app docked beside a shared fact card; honest states and provenance; the same fixture and
Connect lifecycle; governed actions handing off; calm over spectacle — and the round-2 invariants that static
images are not prototypes and every value comes from the fixture.

## 12. Desktop and mobile layouts
**Desktop, 1440 px:** a 232 px shell rail (Home / Connect / Memory / Settings, the visible MOCKUP label,
`#live-toggle`) beside content capped at 1200 px. Home reads: dateline header ("Checked …",
`#simulate-change` as an explicitly labelled mock control) → **the table**, app frame (~62 %) beside the fact
card (~38 %) → **the wall**, tags in a 3-column grid in fixed order with name, owner, state word and
checked-at. `#disconnect` sits in the table header for the selected service, visible on the first screen, and
opens a `role="alertdialog"` whose confirm button reads "Disconnect".
**Tablet, 768 px (bonus):** rail becomes a top bar, 8 columns, wall 2-up.
**Mobile, 390 px:** a four-item bottom bar at 44 px, one column, no sideways scroll; the table reads first
and the wall second, so the card is never pushed under a long register; a sticky header keeps the dateline,
MOCKUP label and `#live-toggle`. Taking a tag opens a full-height sheet — app frame over the card, stacked
permission rows, `#disconnect` pinned in the sheet header — with Escape or Back returning focus to the tag;
Connect is full-screen with a "Step n of 6" marker and publish as the one staged beat.


---
## CRITIQUE.md (another engine)

# v2731 Worktable — critique

1. **The Connect lifecycle is incomplete and internally inconsistent.** Sections 2, 8, and 12 say six steps, but enumerate only discover → credential reference → safe test → preview → publish; the required distinct “choose what to publish” stage is missing. The default third service is Studio Printer, but its fixture state is `unavailable` because its last job failed, not a clean “safe test failed” story; the proposed test recovery does not say what happens to the fixture's existing error or how a user can proceed safely. This makes the core integration path hard to implement and obscures the difference between testing a connection and publishing selected facts. **Fix:** name and define all six stages, including explicit per-fact selection and a publish summary; specify a deterministic failed-test state, retry/recovery, and whether the user can return to preview without discarding the credential reference. Keep Studio Printer's fixture facts and existing error distinct from test results.

2. **The “real daily-use app” stops at Home and Connect.** Section 3 lists Home, Connect, Memory, Settings as persistent destinations, but gives no Settings screen, controls, defaults, or behavior; Memory is only mentioned as a navigation label. The first-run description in section 6 treats zero services as a literal empty table with an add CTA, but does not establish the promised local Worlds/Memory baseline or how a first-time person reaches settings and can return to the shell. “Loading,” error, and disconnect are named, but the disconnect banner lacks a recovery/undo path and the generic test failure lacks fixture-grounded copy. That is not enough to judge an application someone could use every day. **Fix:** specify concrete Settings and Memory destinations with useful ordinary controls/content, initial sensible defaults, and first-run navigation; define the service-drop error using fixture copy and recovery, plus what remains available after disconnect and whether/how reconnect restores it. Do not imply historical retention that the fixture does not establish.

3. **Reactivity is asserted, not designed as the required live behavior.** Section 2's `#simulate-change` is a useful single atomic cross-view example, but it is a manually triggered mock action and does not supply the brief's ongoing feed: the only periodic behavior in sections 10/12 is a “checked” line, visible pause, and `#live-toggle`. A timestamp aging is not an occasional fixture-backed transition to stale, and there is no specified optimistic action/settled result or visible transition for other state changes. The wording also suggests the mock `lock-back-door` changes the external fact even though the fixture says this is mock review only and real approval belongs in Project Home. **Fix:** define a fixture-only tick/event sequence that visibly advances freshness and sometimes marks a named service stale, propagating to Home, selected service, and Agent facts; describe pause/resume and calm settle states. Specify an optimistic state and confirmed/failure rollback for the mock handoff, and label it as simulation so no external action is implied. Honor reduced motion without disabling data updates.

4. **The two layouts and the service-selection model conflict.** Section 12 calls for a table and 3-column wall on desktop while the metaphor and section 3 say the table holds the selected app and card; on mobile the “table reads first” but taking a tag opens a sheet, leaving unclear whether the wall or sheet is the actual selected-service surface. The first screen is also not reliably defined: desktop has a selected service and visible disconnect, but the initial selection is unspecified (section 2 says Front Door is on the table while section 12 places `#disconnect` for the selected service). A mobile full-height sheet is modal, yet the brief's accessibility contract requires focus trap, explicit close, Escape dismissal, and focus return; the proposed Escape/Back return omits these details, and 44px targets/safe areas are not specified for the 44px bottom bar. **Fix:** give 1440px and 390px explicit, consistent reading order, initial selection, first-screen disconnect placement, and clear list/detail transitions. Specify sheet close button, modal focus behavior, safe-area handling, and minimum 44×44 hit areas; clarify how the persistent four destinations coexist with the full-height sheet and preserve no-side-scroll/reflow.

5. **The direction is more named than differentiated, and the OpenDesign use is not credible yet.** Section 11 says it opens C's cabinet and moves the wall below the composition, but retains C's same persistent shell, owned service register, docked owner app plus shared fact card, states, lifecycle, fixture and handoffs. The table/wall vocabulary and “one fact card” signature do not yet establish a materially different interaction or visual thesis; the anti-repeat list is not addressed, so the wall/table could read as a renamed service stage or desk. Section 8 claims `skill-impeccable-design-polish` and `ds-warm-editorial`, but supplies no evidence that those assigned packet choices are authorized for v2731, and its citation to only generic rule names does not show how the skill's audit/critique/polish process or system tokens govern actual shell, controls, and mobile composition. The generic warm palette and one-shot wren are assertions, not the requested specific, restrained warmth across daily use. **Fix:** state the concrete interaction and visual differences from C and name the closest anti-repeat concepts it avoids, then show what the borrowed mechanism contributes. Verify the assigned skill/system for v2731; cite the actual packet and map its rules/tokens to the 1440/390 layouts, controls, and states, including measured contrast. Add one specific warm microcopy/illustration or connected moment tied to fixture behavior, with reduced-motion behavior, while keeping status text explicit and the structure calm.

