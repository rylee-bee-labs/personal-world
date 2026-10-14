# Worlds vNext real round: critique concept v2731's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v2731/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v2731

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

