# Concept brief — `<concept-slug>`

<!--
TEMPLATE. Copy this file to docs/vnext/concepts/<NN>-<concept-slug>.md and fill
every section. Replace every `<...>` placeholder and delete every italic
instruction once answered. A brief with unfilled placeholders is not finished.
Keep the section order; the review reads it top to bottom.

This template is cut from docs/vnext/fixture/concept-brief.md (this file).
It is one of the shared inputs for the 30-concept Worlds exploration; all
concepts fill the same headings so they can be compared fairly.
-->

> **MOCKUP — `<concept-slug>`.** This is a design prototype, not the production
> front door. Nothing here is shipped. Every state is synthetic; see
> **§9 What is invented vs measured**.

| | |
|---|---|
| **Concept** | `<name>` |
| **Slug** | `<concept-slug>` |
| **One-line idea** | `<one sentence: what the concept is and who it is for>` |
| **Fixture** | `docs/vnext/fixture/fixture.json` (shared, constant — do not edit per concept) |
| **Service ids used** | `<e.g. front-door, garden-camera, local-weather>` |
| **Author / date** | `<name / YYYY-MM-DD>` |
| **Prototype path** | `<path to the runnable prototype, e.g. docs/vnext/concepts/<NN>-<slug>/index.html>` |
| **Status** | `<draft / ready-for-review / reviewed>` |

---

## 1. Concept name and one-line idea

**Name.** `<the concept's name>`

**One-line idea.** `<one sentence. Concrete: what a person sees and does first.>`

**Who it is for / when.** `<the moment this concept serves — e.g. a busy morning,
a quiet evening, a screen-reader user checking what needs them>`

**What makes it different from the other 29.** `<one or two sentences. The exploration
is looking for range, not 30 skins of one idea.>`

**Design references it borrows from.** `<name the reusable patterns from
docs/vnext/design-research.md Part 3 (numbers + short names) that this concept uses,
and say what each does here. At minimum, patterns 1, 2, 4, 5, 7, 11 and 12 are
expected somewhere in every concept.>`

---

## 2. Static images are not prototypes

> 2026-10-13 brief.md update 19:20Z: "Static images are not prototypes."

A screenshot, a Figma frame, or a rendered PNG is **not** an acceptable submission
for this section. This concept is accepted only with a **working prototype** whose
interactions can be operated live — by pointer **and** by keyboard alone — against
the shared fixture.

- **Prototype path / URL:** `<path or local URL>`
- **How to run it:** `<exact command or file to open>`
- **What a reviewer must be able to do without reading this brief:** `<list the three
  interactions and the demo tool, since §3–§4 define them>`

---

## 3. The three working interactions

> 2026-10-13 brief.md update 19:20Z: each concept needs "three working interactions
> with one updating several views".

Every concept has exactly **three** working interactions. At least **one** must update
**several views at once** from a single shared fact (design-research pattern 1). Each
must show running → done → failed states in place (pattern 2), and each must be
operable by pointer and by keyboard (pattern 12). Interactions are declared on the
object, like Home Assistant `tap_action` / `hold_action` (pattern 3).

For each interaction, fill this block:

### Interaction 1 — `<short label>`

- **What the person does:** `<the action, in plain words>`
- **What it changes (one fact → several views):** `<name the fixture fact id, e.g.
  frontdoor.back-door, and list every view that re-renders. This is the "updates
  several views" requirement.>`
- **States shown in place:** running `<what shows>` → done `<what shows>` → failed `<what shows>`
- **Permission check:** `<which permission gates it; who may do it; what a disallowed
  attempt shows — see §5>`
- **Provenance effect:** `<how the checked-at time / source changes after the action>`
- **Pointer path:** `<tap/click target>`
- **Keyboard path:** `<keys, focus order, visible focus>`

### Interaction 2 — `<short label>`

- **What the person does:** `<...>`
- **What it changes:** `<...>`
- **States shown in place:** running `<...>` → done `<...>` → failed `<...>`
- **Permission check:** `<...>`
- **Provenance effect:** `<...>`
- **Pointer path:** `<...>`
- **Keyboard path:** `<...>`

### Interaction 3 — `<short label>`

- **What the person does:** `<...>`
- **What it changes:** `<...>`
- **States shown in place:** running `<...>` → done `<...>` → failed `<...>`
- **Permission check:** `<...>`
- **Provenance effect:** `<...>`
- **Pointer path:** `<...>`
- **Keyboard path:** `<...>`

**Which interaction updates several views (required).** `<name it and quote the
fixture's crossViewExample if you used it>`

---

## 4. Embedded demo tool plus fallback

> 2026-10-13 brief.md update 19:20Z: each concept needs "an embedded demo tool plus
> fallback". Design-research patterns 7, 9, 13.

- **The demo tool:** `<what it is and what it does — a working tool that lets a
  reviewer operate the concept's data, e.g. a visual editor plus the raw config it
  produces (pattern 9)>`
- **What "embedded" means here:** `<how the tool sits inside the concept, and what
  boundary keeps it safe (pattern 8 / MCP ext-apps sandbox)>`
- **The fallback:** `<the plain view shown when the tool is unavailable, disabled, or
  a provider is absent. This must be a real state a reviewer can reach, not prose.>`
- **How a reviewer triggers the fallback:** `<exact action, e.g. the "disable demo"
  toggle, or running with no provider configured>`
- **Zero-provider usability:** `<confirm the concept still shows its state and its
  three interactions with no external provider configured (protected invariant).>`

---

## 5. Agent-visible shared facts and separate permissions

> 2026-10-13 brief.md update 19:20Z: each concept needs "agent-visible shared facts
> with separate permissions". Design-research patterns 1, 5, 8, 10.

The agent and the person must be able to see the **same fact** (the embedded view is
the agreed rendering of it — pattern 8), while permissions differ per fact and per
principal. The fixture declares an `agent` and a `human` permission on every fact;
this concept must honour them.

- **Facts the agent may read:** `<fixture fact ids where permissions.agent == "read">`
- **Facts the agent may act on:** `<ids where permissions.agent == "act", or none>`
- **Facts the agent may NOT see:** `<ids where permissions.agent == "none"; these must
  render as a named, disabled control with the reason, never hidden (pattern 10)>`
- **Facts the person may read / write / act on:** `<list by permission>`
- **How the difference is shown on screen:** `<e.g. an "agent view" preview, a disabled
  control with reason text, a shared-fact badge>`
- **Host-mediated action record:** `<how an action requested from an embedded surface
  is checked and the result recorded (pattern 8)>`
- **Security / consent:** `<what consent is required before the agent acts, and how the
  person can refuse (protected invariant)>`

---

## 6. How stale / unknown / provenance is shown

> 2026-10-13 brief.md update 19:20Z: "honest provenance/stale/UNKNOWN". Design-research
> patterns 4 and 5. Protected invariant: no invented status; external systems keep
> their own authority.

The fixture's services cover every state the concept must render honestly:

| Fixture service | State | What the concept must show |
|---|---|---|
| `media-shelf` | `healthy` | `<...>` |
| `front-door` | `needs_attention` | `<...>` |
| `nightly-backup` | `stale` | `<...>` |
| `garden-camera` | `unknown` | `<...>` |
| `studio-printer` | `unavailable` (retry-able) | `<...>` |
| `notes-sync` | `unavailable` (retry-able) | `<...>` |
| `local-weather` | `not_configured` (zero facts) | `<...>` |

Answer all of these:

- **State vocabulary used:** `<the exact labels from ACCESSIBILITY_CONTRACT §1.3 —
  healthy, needs_attention, unavailable, stale, unknown, disabled, not_configured.
  Do not invent synonyms.>`
- **No colour-alone status:** `<how each state also carries a text label (floor §1.3–1.4)>`
- **Stale shown as stale:** `<how an old checked-at time is surfaced rather than
  presenting old data as live>`
- **Unknown shown as unknown:** `<how a missing observation renders as unknown/
  unavailable, never as a blank or a zero (pattern 4)>`
- **Provenance on every external fact:** `<where checked-at + source appear; source is
  'local', 'external' or 'none' (pattern 5)>`
- **Retry-able failures:** `<how the two failed items show their last error and offer
  retry, and what retry's running/done/failed states look like>`
- **Zero-data item:** `<how local-weather renders with no facts at all — honest empty
  state, no placeholder value>`
- **No invented status:** `<confirm the concept never guesses a value or a healthy
  state it did not observe (protected invariant)>`
- **External authority:** `<confirm the owning app keeps authority; the tile is a view,
  and changes go through the owning app / host (fixture ownerFallback)>`

---

## 7. Phone and keyboard behaviour

> 2026-10-13 brief.md update 19:20Z: "working phone/keyboard variant". Design-research
> patterns 11 and 12. Floors: ACCESSIBILITY_CONTRACT §2 and RESPONSIVE_RULES.

- **Breakpoint / reflow:** `<how the layout collapses. The repo's one breakpoint is
  860px; multi-column views collapse to a single column with the same order of actions
  (pattern 11).>`
- **Order preserved on phone:** `<confirm the reading/action order is unchanged from
  desktop to phone; CSS reorders nothing semantically (floor §5.4)>`
- **Safe areas:** `<how controls clear notches and the home indicator>`
- **Targets:** `<every interactive target ≥44×44 CSS px at every breakpoint (floor §2.1)>`
- **Keyboard reaches every action:** `<tab order for all three interactions + the demo
  tool + the fallback; confirm every pointer action has a keyboard path (floor §2.2,
  pattern 12)>`
- **Visible focus:** `<focus indicator, 2px, and its contrast (floor §2.4)>`
- **No hover/drag/precision required:** `<confirm, or name the button/menu alternative
  (floor §2.3)>`
- **Live-region restraint:** `<what is announced on a state change; nothing per poll
  (floor §8)>`
- **Reduced motion:** `<confirm motion defaults to reduced; nothing essential animates
  (floor §6.2)>`
- **200% zoom / reflow:** `<confirm no horizontal page scroll, clipping or truncation
  (floor §6.4)>`

---

## 8. MOCKUP labelling

- **Where the MOCKUP label appears:** `<the visible label on the prototype itself —
  persistent, not a comment in the source>`
- **Wording used:** `<exact text, e.g. "MOCKUP — synthetic data, not the live world">`
- **What it labels:** `<which parts are mock: data, chrome, both>`
- **How it survives export:** `<if a screenshot is taken, the label is in the frame>`

---

## 9. What is invented vs measured

> Protected invariants: no invented status; honest provenance. This section is the
> concept's own provenance. Be explicit; an unmarked invention is a defect.

| Claim in this brief | Invented / measured | Basis |
|---|---|---|
| `<the concept idea>` | invented | `<design intent>` |
| `<layout / interaction>` | `<invented or measured>` | `<built and run, or intent only>` |
| `<any data value shown>` | invented | synthetic fixture `docs/vnext/fixture/fixture.json` |
| `<any accessibility claim>` | measured | `<test command and result, e.g. keyboard walkthrough, axe run>` |
| `<any claim about an external design reference>` | `<UNKNOWN unless verified>` | `<source checked, or UNKNOWN>` |

- **Design-library note.** `DESIGN-QUALITY-GUIDE.md` and `PERFECT-PROMPT-TEMPLATE.md`
  were **not readable from this host** (the design media library path does not exist
  here; see `docs/vnext/design-research.md` §1a). Any quality-bar claim sourced from
  them is `UNKNOWN`. The quality bar used here comes from the readable canonical
  in-repo references listed below.
- **Quality bar actually applied:** `<ACCESSIBILITY_CONTRACT.md (floor),
  RESPONSIVE_RULES.md, design/tokens.json (state vocabulary + tokens),
  docs/vnext/design-research.md Part 3 patterns.>`
- **Test command and result:** `<exact command → observed result>`
- **Anything not verified:** `<list, or none>`

---

## 10. Decisions in force (do not contradict)

Quoted from `docs/vnext/design-research.md`; these are the owner's decisions, not
suggestions. Nothing in this concept may contradict them.

- 2026-10-13 Rylee comment 6153184069 on #322: the three draft directions were "too
  unimaginative"; a wider 30-concept exploration with working interactions comes
  before choosing.
- 2026-10-13 brief.md update 19:20Z: "Static images are not prototypes."
- 2026-10-13 brief.md update 19:20Z: each concept needs "three working interactions
  with one updating several views, an embedded demo tool plus fallback, agent-visible
  shared facts with separate permissions, honest provenance/stale/UNKNOWN, working
  phone/keyboard variant".
- 2026-10-13 brief.md Authority: protected invariants are local Memory, accessibility
  floors, zero-provider usability, security/consent, no invented status, and external
  systems keep their own authority.

**Self-check.** `<confirm the concept contradicts none of the above. If it seems to
need to, stop and name which decision is in the way rather than working around it.>`

---

## 11. Accessibility checklist (floor — tick or mark UNKNOWN)

- [ ] Status never by colour alone; every state has a text label (floor §1.3–1.4)
- [ ] Loading indicators static; no shimmer/pulse (floor §1.5)
- [ ] All targets ≥44×44 CSS px (floor §2.1)
- [ ] Every function operable by keyboard alone (floor §2.2)
- [ ] No essential hover/drag/double-click/right-click (floor §2.3)
- [ ] Focus always visible, 2px, sufficient contrast (floor §2.4)
- [ ] Real heading hierarchy, no skipped levels (floor §4.1)
- [ ] Landmarks: header, navigation, main, complementary (floor §4.2)
- [ ] Drawers/sheets/dialogs follow floor §3 (focus, Escape, return)
- [ ] Live-region restraint: no per-poll announcements (floor §8)
- [ ] Motion defaults to reduced (floor §6.2)
- [ ] Survives 200% zoom/reflow without horizontal scroll (floor §6.4)

---

## 12. Review evidence

- **Prototype to operate:** `<path / URL>`
- **Keyboard-only walkthrough:** `<transcript or steps → result>`
- **Phone-width check:** `<widths tested and what reflowed>`
- **Screenshots (labelled MOCKUP):** `<paths; screenshots are evidence, not the prototype>`
- **Tests run for what changed:** `<command → observed result>`
- **Reviewer sign-off:** `<name / date / status>`
