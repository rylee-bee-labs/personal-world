# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c498

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): gpt-taste totality-festival — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c498/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c498/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

# DESIGN.md — c498 · Worlds as an eclipse observatory

**Status:** MOCKUP design decision record, not approved. No code in this step; after it,
`index.html` sits beside this file. **Starting point:** OpenDesign skill
[`gpt-taste`](../../od-pack/skill-gpt-tasteskill.md) + design system
[Totality Festival](../../od-pack/ds-totality-festival.md). Context: [concept brief v2](../../concept-brief-v2.md),
[fixture](../../fixture/fixture.json), [design research](../../design-research.md).

## 1. Metaphor in one line, and the main gesture

Worlds is an **eclipse observatory**: services are celestial bodies on one dark field, and
the shell is the deck you watch them from. The main gesture is **pull a body into the
corona** — focus one service and its glass panel expands to the hero while the rest recede
to labelled points. One focus recomposes the whole field: the focused panel, the attention
count, the Shared Chart, and the docked app together. The field never animates at the user;
motion only confirms a change the user already made.

## 2. Integration model

A service plugs in as a **body entering the field**, following the Connect lifecycle shape
from the Connect contract draft (#329): **discover** (a new point appears in the empty
field), **credential reference** (a *beacon id* — a reference only, never a secret value),
**safe test** ("signal check" with visible running / done / failed states), **preview** (a
**chart** of the facts and capabilities the body offers), **choose what to publish** (which
instruments to mount), after which the body is placed on the field and its facts are charted.

The field composes three fixture services: **Media Shelf** (`media-shelf`) and **Nightly
Backup** (`nightly-backup`) arrive in view; the third is added live in the prototype
(**Garden Camera**, `garden-camera`), whose signal check honestly returns **no answer**. Each
service owns its data and actions; the shell composes. Every fact becomes a charted line with
its `source`, `checkedAt` (age derived) and its two permission columns.

## 3. Navigation between Worlds and a service

**Desktop:** a floating glass nav pill (Worlds · Connect · Memory · Settings) over the field;
a **pinned rail** holds the focused body's detail while the field of bodies stays live beside
it — no scroll-jacking, no full-page takeover. `Tab` and arrow keys move focus across the
bento in DOM order; `Enter` focuses a body. **Phone (390 px):** the bento collapses densely
(`grid-auto-flow: dense`) to a single column with no horizontal scroll; the pinned rail
becomes a bottom sheet, non-modal and dismissible with Escape/close, returning focus to the
body that opened it.

## 4. The embedded-app frame

An embedded app is a **docked glass viewport**: the owning app's own UI renders inside a
bordered glass panel with a cyan interactive rim and a status bar naming the owner. Sandbox
stated: the docked app calls back through the host only and cannot write the Shared Chart.
The shell keeps its nav pill, the focused rail, the attention count and a caption ("the shell
frames this app; it does not restyle it") around it. The owning app is one link away —
**Open Media app ↗** (`app://media`, rendered inert as `https://example.invalid/media`) — and
**"Simulate can't embed"** swaps the viewport for a fallback naming the reason and keeping
the link.

## 5. The agent/human shared-facts model

The **Shared Chart** is a glass table every reader sees. Each fact line shows its `source`,
`checkedAt` and derived age, and two **separate** columns — **Agent** and **Human**
(`read` / `act` / `write` / `none`). A reader toggle shows the agent's chart: the same facts,
but `frontdoor.guest-code-used` reads "a guest code was used" with no who (its `agent: none`
plus reason), and `frontdoor.back-door`'s lock is a **named, disabled** control for the agent
with its reason. A decision is never made in the shell: the amber "Lock back door" **hands
off to Project Home**; the printer retry hands off to Studio. The fixture's
`frontdoor.back-door` → `lock-back-door` dependency drives one change across the focused
panel, the attention count, and the Chart at once.

## 6. States

- **Empty / first run** (zero services, zero providers): the field is empty — "No body has
  answered; nothing is in view" — with the Connect steps, and `local-weather` as the built-in
  **Not configured** example, holding zero data.
- **Quiet:** `media-shelf`: "Healthy", 1,284 items, indexing current.
- **Needs-you:** `front-door` — "Needs a human"; the amber **corona flash** (capped at three
  uses per screen).
- **Stale:** `nightly-backup` — "Stale", last success 2026-10-09, destination unreachable.
- **Unknown:** `garden-camera` — "Unknown", source `none`, **no value rendered**; its corona
  arc is absent, and the words say why.
- **Unavailable:** `studio-printer` / `notes-sync` — "Unavailable", retry handed off.
- **Disconnected** (after removing a body): the field says plainly what left — "Garden Camera
  left the field; its facts are no longer charted." Worlds still works.

States are words **plus a shape** (full corona arc / ring / broken arc / empty ring), never
colour alone; amber marks only the human's one decision, cyan only interactive states. A
visible **MOCKUP** label sits in the nav pill, and the Chart states that nothing is a real
reading. Floors held: 390 px no sideways scroll, 44 px targets, keyboard reach with visible
focus, `prefers-reduced-motion`, no restyle of the old FdApp.

## 7. Borrowed, and deliberately different

- **Homarr** — a panel is a view over another system's data; but the field is spatial, not a
  tile grid.
- **Appsmith** — bindings as a reactive graph; reused so one action moves several panels, and
  the signal check is a named action with a run state.
- **HA cards** — explicit `unavailable` / `unknown` states and primary/secondary/tertiary
  actions per object; reused directly.
- **MCP ext-apps** — sandboxed, host-mediated embedding; reused as the dock.
- **Deliberately different:** no scroll choreography in the working canvas; glass expresses
  *which body owns authority* (layering), not decoration; the corona highlight is capped.

## 8. Starting point and guide rules

Totality Festival supplies the tokens (obsidian `#121318`, glass at
`rgba(52,52,58,0.2)` + 20px blur, amber corona `#fff6df` / `#ffd700`, cyan `#bdf4ff` /
`#00e3fd`, Space Grotesk + Inter, 12-column, radius `lg`/`xl`); `gpt-taste` supplies the
method — gapless dense bento, **two-line hero**, huge section spacing, a deterministic
layout choice so the field does not repeat one shape, no cheap meta-labels, legible buttons.
Applied rules: [color](../../od-pack/color.md) (neutrals 70–90%, amber ≤ 3 uses/screen, no
pure black/white), [typography](../../od-pack/typography.md) (tight display tracking, ALL-CAPS
labels ≥ 0.06em, ≤ 2 families), [anti-ai-slop](../../od-pack/anti-ai-slop.md) (no emoji, no
invented metrics, no external placeholder images), [laws-of-ux](../../od-pack/laws-of-ux.md)
(Hick's ≤ 3–5 primaries, chunking, selective attention reserved for one action),
[state-coverage](../../od-pack/state-coverage.md) (five states + signal-check run state +
retry backoff), [accessibility-baseline](../../od-pack/accessibility-baseline.md) (WCAG 2.2 AA,
focus-visible, labels, landmarks), and [animation-discipline](../../od-pack/animation-discipline.md)
(150 ms confirmation, no loop without a pause control, `prefers-reduced-motion`).

## 9. Signature element, hero thesis, reference lock

**Signature:** the **corona ring** around the focused body — an orbital arc whose *length* is
the fact's freshness: a live fact's arc closes, a stale fact's arc stops short, an unknown
fact has no arc. **Hero thesis:** the hero is the one body that needs the human — a single
amber corona around Front Door reading "Needs a human". **Reference lock:** **NASA's Eyes**
(orbital data rings around a focused body) and **Home Assistant's Lovelace entity cards**
(states as first-class, non-normal states rendered distinctly) — mechanics only, not palettes.

## 10. Risks and UNKNOWNs

- **Skill vs. product.** `gpt-taste` is a marketing-landing skill (AIDA, scroll pinning,
  text scrubbing, perpetual hover). `animation-discipline` calls hero choreography in a
  productivity tool a mistake, and the brief bans motion on by default. Taken: the *layout*
  discipline (bento density, two-line hero, no meta-labels); refused: the scroll show. The
  "inline typography image" becomes a CSS-drawn ring, because external images are banned.
- **Font conflict.** `gpt-taste` bans Inter; Totality Festival mandates Inter for body.
  Lever 1 binds the design system, so Inter stays and the ban is recorded.
- **Stack.** GSAP needs a framework/CDN, which the stack rule forbids; the build step must
  hand-code equivalent transitions in vanilla CSS/JS. The layout choice is a design-time
  decision, not a runtime randomiser.
- **Contrast.** The DS glass (`rgba(52,52,58,0.2)` over `#121318`) may not clear 4.5:1 for
  body text; the exact measured values are **UNKNOWN** until built, and text contrast must be
  raised if it fails.
- **Connect lifecycle details** beyond the brief's shape are **UNKNOWN** in this session.
- This is a **new** concept, not an evolution of `patchbay` / `nowline` / `altitude`; the
  nearest round-1 relative, `orrery`, is on the anti-repeat list and is deliberately not
  reproduced.

---
## CRITIQUE.md (by another engine)

# Critique — c498

1. **The observatory repeats a banned round-one concept.** The focused-body field, orbit/corona rings, and explicit “nearest round-1 relative, `orrery`” are too close to the anti-repeat list’s galaxy/star maps and orrery of orbits; calling it a deliberate non-reproduction does not make the interaction distinct. This undermines the required brand-new design. **Fix:** replace the celestial field and orbital focus gesture with a visibly different composition and signature artifact.
2. **The example services and fixture states are altered or invented.** The prototype says Media Shelf and Nightly Backup arrive, Garden Camera is added live, yet Garden Camera is `unknown`, not a newly connected service; `front-door` and `studio-printer` also appear in state descriptions without a consistent initial three-service composition. Section 2 further invents Garden Camera as an added service (which is in the fixture, but the brief requires the third to be added through lifecycle, not assert a test result as a service state). **Fix:** start with two clearly specified fixture services, add a third fixture service through the full lifecycle, and preserve each fixture ID, name, and state verbatim; show test outcome separately from service state.
3. **The core shell composition is asserted, not specified end-to-end.** “One focus recomposes” the panel, attention count, Shared Chart, and docked app, but no concrete interaction establishes the home surface, service-owned surface, and agent-facts view updating together. Nor does the design define the required disconnect action/confirmation and post-removal behavior beyond a static sentence. This risks returning to concepts-on-a-page rather than an integrated shell. **Fix:** name the controls and state transition that update all three views, and define how the user disconnects a service and sees its removal reflected.
4. **Permission columns do not answer the required questions.** “Agent” and “Human” values combine `read` / `act` / `write` / `none`, while the requirement asks separately who can see and who can act. **Fix:** present visibility and action authority independently for both readers, preserving fixture reasons for disabled controls.
5. **The OpenDesign method is selectively overridden without a resolved system.** Totality Festival calls for cosmic gradients/neon-like accents and its glass/amber/cyan palette conflicts with the accessibility floor’s no-neon rule; the design also changes the system’s Inter choice to Space Grotesk and admits contrast is unknown. **Fix:** pin a compliant token/font decision within the assigned system, resolve the contrast conflict before build, and name the concrete guide decisions that survive those constraints.
