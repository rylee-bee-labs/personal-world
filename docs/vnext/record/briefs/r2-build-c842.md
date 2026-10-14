# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c842

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): minimalist-ui paper — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c842/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c842/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

# DESIGN.md — c842 · Worlds as a librarian's index sheet

**Status:** MOCKUP design decision record, not approved. No code in this step; after it,
`index.html` sits beside this file. **Starting point:** OpenDesign skill
[`minimalist-ui`](../../od-pack/skill-minimalist-skill.md) + design system
[Paper](../../od-pack/ds-paper.md). Context: [concept brief v2](../../concept-brief-v2.md),
[fixture](../../fixture/fixture.json), [design research](../../design-research.md).

## 1. Metaphor in one line, and the main gesture

Worlds is a **librarian's index sheet**: one quiet, continuous document in which every service
is a line item. There is no page to visit and no field to move across; the main gesture is
**unfurl a line** — expand a service row in place so its facts, its embedded app and its two
readers open *within* the document, then close again. The shell is the paper, not a
destination.

## 2. Integration model

A service plugs in as a **new entry being filed**, following the Connect lifecycle shape from
the Connect contract draft (#329): **discover** (a candidate is listed), **credential
reference** (a *catalogue reference* — a reference only, never a secret value),
**safe test** ("verify the reference answers", with visible running / done / failed states),
**preview** (a proof of the facts it will file), **choose what to publish** (which columns to
file), after which the entry appears in the index. The index composes three fixture services:
**Media Shelf** (`media-shelf`) and **Front Door** (`front-door`) are filed; the third is
filed live in the prototype (**Local Weather**, `local-weather`), which honestly reports
**Not configured** and files zero facts. Each service owns its data and actions; each filed
fact becomes a line under its service with its `source`, `checkedAt` (age derived) and its two
permission columns.

## 3. Navigation between Worlds and a service

**Desktop:** the whole shell is one column (`max-width: 65ch`–`80ch`), with a sticky index bar
of service names that anchors to each row; there is no route change — unfurling a row expands
it in place, and the document scrolls naturally. `Tab` reaches each row's toggle; `Enter`
expands; `Escape` collapses and returns focus to the row head. **Phone (390 px):** the same
single column, the index bar wraps within its own container (no page-level sideways scroll),
and expanded detail stacks beneath its line.

## 4. The embedded-app frame

An expanded row contains a **framed app**: a flat white panel with a minimalist window chrome
(three small light-gray circles in a 1px `#EAEAEA` top bar, per the skill's faux-OS convention)
holding the owning app's own UI. Sandbox stated: the framed app calls back through the host
and cannot edit the index. The shell keeps the index bar, the row heading, the state word and
the "open in the owning app" fallback around it. The owning app is one link away — **Open Home
app ↗** (`app://home`, rendered inert as `https://example.invalid/home`); **"Simulate can't
embed"** swaps the panel for a plain fallback row naming the reason and keeping the link.

## 5. The agent/human shared-facts model

Each fact line carries a monospace provenance line (`source local · checked 14:23:58Z · age
28s`) and two **separate** reader marks: **Agent: read / act / none** and **Human: read /
write / act / none**. A **reader** toggle re-renders the same ledger as the agent sees it:
identical facts, but `frontdoor.guest-code-used` reads "a guest code was used" with no who
(its `agent: none` plus reason), and a control the agent lacks is a **named, disabled** control
with the fixture's reason — never hidden. A decision is never made in the sheet: "Lock back
door" **hands off to Project Home** (approvals); the printer's retry hands off to Studio. One
change in one service updates the index summary, that service's row, and the agent's view at
once — the fixture's `frontdoor.back-door` → `lock-back-door` → `front-door: healthy`,
`attentionCount: 0`.

## 6. States

- **Empty / first run** (zero services, zero providers): "The index is empty. No service has
  been filed." — a headline, a plain explanation and the Connect steps as the single next
  action; `local-weather` is the built-in **Not configured** example.
- **Quiet:** `media-shelf`: "Healthy", 1,284 items, indexing current.
- **Needs-you:** `front-door` — "Needs a human"; the one dark `#111111` emphasis mark.
- **Stale:** `nightly-backup` — "Stale", last success 2026-10-09, destination unreachable.
- **Unknown:** `garden-camera` — "Unknown", source `none`, **no value rendered**, only the
  word and the reason.
- **Unavailable:** `studio-printer` / `notes-sync` — "Unavailable", retry handed off.
- **Disconnected** (after removing a service): a struck-through line — "Front Door was removed
  on <date>; its facts are no longer read." Worlds still works.

States are words **plus a mark** (solid rule / dot / broken rule / absent rule), never colour
alone. A visible **MOCKUP** label sits in the index bar, and the sheet states that nothing is
a real reading. Floors held: 390 px no sideways scroll, 44 px targets, keyboard reach with
visible focus, `prefers-reduced-motion`, no restyle of the old FdApp.

## 7. Borrowed, and deliberately different

- **Dashy** — configuration as a reviewable document; taken furthest: the index *is* the
  document, and it is a quiet sheet, not a start page of tiles.
- **Homarr** — a row is a view over another system's data, not the data itself.
- **HA cards** — the explicit state set and per-object actions; reused, rendered as words.
- **MCP ext-apps** — sandboxed, host-mediated embedding; reused as the framed app.
- **Appsmith** — a named action with a run state; reused for the Connect verification.
- **Deliberately different:** no bento-dashboard default and no spatial field — detail is
  progressive disclosure inside one linear document; the only "cards" are the flat 1px-bordered
  blocks inside an unfurled row.

## 8. Starting point and guide rules

Paper supplies the tokens (primary `#111111`, text `#111827`, surface `#FFFFFF`, scale
14/16/18/24/32/40, spacing 4/8/12/16/24/32); `minimalist-ui` supplies the method — warm
monochrome, flat bento, no gradients or heavy shadows, `1px solid #EAEAEA` borders, 8–12px
radii, macro whitespace, `<kbd>`-style details, muted pastels for tags only. Applied rules:
[color](../../od-pack/color.md) (neutrals 70–90%, one accent, contrast gates, no pure black),
[typography](../../od-pack/typography.md) (three-weight system, ALL-CAPS tracking ≥ 0.06em,
50–75 ch), [anti-ai-slop](../../od-pack/anti-ai-slop.md) (no emoji, no invented metrics, no
filler copy), [laws-of-ux](../../od-pack/laws-of-ux.md) (Miller chunking, proximity vs common
region, progressive disclosure for Tesler's Law), [state-coverage](../../od-pack/state-coverage.md)
(five states + form states + retry backoff), [accessibility-baseline](../../od-pack/accessibility-baseline.md)
(WCAG 2.2 AA, focus-visible, labelled inputs, landmarks), and
[animation-discipline](../../od-pack/animation-discipline.md) (scroll-entry fade via
`IntersectionObserver` only, ≤ 160 ms, reduced-motion honoured, never the only state signal).

## 9. Signature element, hero thesis, reference lock

**Signature:** the **ledger rule** — a 1px `#EAEAEA` line under each fact that doubles as the
freshness axis: solid to "now" for a live fact, stopping short for a stale fact, absent for an
unknown one, beside one monospace provenance line. **Hero thesis:** the hero is the ledger's
first line — one fact, its source, its age, and its two readers, stated plainly. **Reference
lock:** **Linear's list views** (a row is an object; detail opens in place; hairline dividers)
and **Notion databases** — mechanics only, not palettes.

## 10. Risks and UNKNOWNs

- **Font conflict (recorded, not hidden).** `minimalist-ui` bans Roboto; Paper mandates it for
  body. Lever 1 binds the assigned design system, so the prototype follows Paper
  (Montserrat display, Roboto body, PT Mono) and records the ban as a review flag.
- **Banned token.** Paper's secondary `#8B5CF6` is on the P0 anti-ai-slop indigo list. It is
  **not used**; the single accent is Paper's primary `#111111`. This is a deliberate deviation
  from the DS palette, flagged.
- **Type scale ceiling.** Paper tops out at 40px, while the skill wants an editorial serif
  hero; the hero must stay ≤ 40px, so it is a *line*, not a display headline — recorded.
- **Radii.** Paper declares no radius token; the 8px value comes from the skill and is flagged.
- **No external fonts.** The stack rule bans CDN fonts, so Montserrat / Roboto / PT Mono render
  through system fallbacks; the real faces are **UNKNOWN** offline.
- **Document length.** One continuously expanding index could get very long; progressive
  disclosure is the mitigation, but the phone reading experience is **UNKNOWN** until built.
- **Window chrome.** The three decorative circles must be `aria-hidden` so they are not
  announced, and must not imply a real macOS window.
- **Connect lifecycle details** beyond the brief's shape are **UNKNOWN** in this session.
- This is a **new** concept, not an evolution of `patchbay` / `nowline` / `altitude`; the
  nearest round-1 relative, `the-desk`'s plain-text surface, is on the anti-repeat list and is
  deliberately not reproduced.

---
## CRITIQUE.md (by another engine)

# Critique — c842

1. **The defining concept copies a banned round-one surface.** The “one quiet, continuous document,” plain line items, and “hero is the ledger’s first line” closely reproduce the anti-repeat item “plain-text desk”; the stated distinction of progressive disclosure does not change the core visual or interaction model. **Fix:** replace the continuous index-sheet metaphor and line-unfurl gesture with a distinct composition and signature element.
2. **The embedded-app owner and fallback are wrong for the stated frame.** The frame is placed in an expanded service row without naming that service, but its link points to `app://home` / `Open Home app`, which is not a valid fixture `ownerFallback` URL (`front-door` owns Home, while other services have their own owners). The brief requires an owning service’s UI framed inside Worlds and shell navigation/back/status to keep working. **Fix:** bind the frame to a named fixture service, use its exact owner/fallback route rendered as `https://example.invalid/...`, and specify persistent shell navigation/status and host-mediated boundary.
3. **The reactive shared-facts behavior is only promised, not concretely connected.** It claims a change updates an “index summary,” row, and agent view, but does not define a home surface or an interaction/state transition demonstrating the same fact at once across home, that service’s own surface, and agent-facts view. The lock example also states a resulting healthy state/count without clarifying it is a mock transition rather than changed fixture data. **Fix:** define the three visible surfaces and their shared state binding, and label any simulated result while preserving the fixture baseline.
4. **Permissions are not represented as separate visibility and action rights.** “Agent: read / act / none” and “Human: read / write / act / none” put different permission dimensions in one mark. **Fix:** show who can see and who can act as separate fields for agent and human; keep each `none` as a named disabled control with its fixture reason.
5. **The OpenDesign starting point is not faithfully resolved.** Paper assigns `#111111`, which is pure black and conflicts with the cited color rule’s no-pure-black floor; the design retains it as its only accent. It also adopts faux-OS window chrome despite the concept’s minimalist document direction and leaves fonts to system fallbacks. **Fix:** resolve Paper tokens against the craft rules without introducing unassigned values, remove unnecessary chrome, and pin a stack-compliant typography choice before build.
