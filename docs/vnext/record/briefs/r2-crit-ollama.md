# Worlds vNext round 2, step 2: critique three DESIGN.md files (another author's)

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- Never one-shot: every design gets a critique by a different engine before it is built. You are that critic.

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. Below are three DESIGN.md files by another author. For EACH, write `docs/vnext/round2/<concept-id>/CRITIQUE.md` with AT MOST 5 gaps, most important first. Each gap: what is missing or wrong against the brief (cite the requirement number or floor), why it matters, and the concrete fix. Focus on integration (plug-in live, composition, embedded frame, one fact many places, governed handoff, disconnect), honesty (stale/unknown, no invented data), uniqueness vs the anti-repeat list, and whether the OD skill/design system was really used. Do not rewrite the design; do not grade generously; if a design has fewer than 5 real gaps, list fewer. Write only the three CRITIQUE.md files. Do not push.


---
## DESIGN.md for c498

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
## DESIGN.md for c572

# DESIGN.md — c572 · Worlds as a printed folio

**Status:** MOCKUP design decision record, not approved. No code in this step; after it,
`index.html` sits beside this file. **Starting point:** OpenDesign skill
[`stitch-design-taste`](../../od-pack/skill-stitch-skill.md) + design system
[Atelier Zero](../../od-pack/ds-atelier-zero.md). Context: [concept brief v2](../../concept-brief-v2.md),
[fixture](../../fixture/fixture.json), [design research](../../design-research.md).

## 1. Metaphor in one line, and the main gesture

Worlds is a **bound folio**: a printed catalogue of what it knows, one numbered plate per
service, with running heads, folio numbers, and Roman-numeral section walks. The main
gesture is **turn to a plate** — leafing through the folio. One turn recomposes the whole
spread at once: the contents rail, the open plate, the running folio (`x / 008`), and the
shared Index below it. Nothing is hidden by being off-plate; turning only changes *which*
page is open.

## 2. Integration model

A service plugs in as a **commissioned plate**, following the Connect lifecycle shape from
the Connect contract draft (#329): **discover** (a candidate appears in the unbound
listing), **credential reference** (a *commission number* — a reference only, never a
secret value; the folio stores and shows the reference, not the key), **safe test** (a
"proof pull" with visible running / done / failed states), **preview** (a proof sheet of
the facts and capabilities the service offers), **choose what to publish** (which fact
lines are filed into the catalogue), then the plate is **bound** with a Roman numeral and
a plate number.

The folio composes three fixture services: **Media Shelf** (`media-shelf`) and **Front
Door** (`front-door`) arrive bound; the third is commissioned live in the prototype
(**Studio Printer**, `studio-printer`). Each service owns its data and actions; the folio
is only a view over them. Every published fact becomes a stamped **Index line** carrying
its `source`, its `checkedAt` (age derived, never asserted), and its two permission
columns.

## 3. Navigation between Worlds and a service

**Desktop:** a fixed left contents rail (Roman numeral · service name · state word), the
folio spread in the centre, and a right margin column for provenance and agent notes. The
rail is a list of links; `←` / `→` leaf through plates, `Home` / `End` jump to the ends.
**Phone (390 px):** the spread is a single column; the contents rail becomes a sticky
strip of plate numbers that scrolls *within its own container* (no page-level sideways
scroll); the right margin folds under each plate. Selecting a service never leaves the
folio — it turns to that plate, and Worlds' running heads and status stay in view.

## 4. The embedded-app frame

An embedded app is drawn as a **plate with corner brackets and a plate number** (Atelier
Zero's image annotation): the owning app's own UI renders inside a frame the shell does not
restyle, on a stated sandbox — it calls back through the shell host, and it cannot write the
Index. The shell keeps around it the contents rail, the running heads, a state line, and a
hairline rule; a caption names the owning app and its route. The owning app is always one
link away — **Open in the owning app ↗** — using the fixture's own route (`app://studio/printers`),
rendered inert in the browser as the reserved `https://example.invalid/studio/printers`. A
**"Simulate can't embed"** control swaps the plate for a fallback that names the reason and
keeps the same link.

## 5. The agent/human shared-facts model

The folio's **Index (colophon)** is one list of facts read by two readers. Each line shows
the fact, its `source`, its `checkedAt` and derived age, and two **separate** columns:
**Agent may** and **Human may** (`read` / `act` / `write` / `none`, from the fixture). A
**lens** toggles *Your copy* / *Agent's copy*: the fact lines are identical, but
`frontdoor.guest-code-used` reads "a guest code was used" for the agent (its `agent: none`
plus reason) and names the person for the human. A control the reader lacks is a **named,
disabled** control with the fixture's reason, never hidden. A decision is never made here:
"Lock back door" does not act — it **hands off to Project Home** (approvals) where the human
holding `act` decides; the printer's "Retry last job" hands off to Studio. One change in one
service updates home, that service's plate, and the Index together — the fixture's
`frontdoor.back-door` → `lock-back-door` → `front-door: healthy`, `attentionCount: 0`.

## 6. States

- **Empty / first run** (zero services, zero providers): an **unbound folio** — one blank
  bracket outline, the words "No plates are bound yet", and the Connect steps as the single
  next action. `local-weather` is the built-in worked example of **Not configured**.
- **Quiet:** all plates live (`media-shelf`: "Healthy", 1,284 items, indexing current).
- **Needs-you:** `front-door` — the word "Needs a human"; this is the folio's **single coral
  moment**.
- **Stale:** `nightly-backup` — "Stale", last success 2026-10-09, destination unreachable.
- **Unknown:** `garden-camera` — "Unknown", source `none`; **no value is rendered**, only
  the word and the reason ("stopped answering at the baseline").
- **Unavailable:** `studio-printer` / `notes-sync` — "Unavailable" with the retry handed off.
- **Disconnected** (after removing a plate): a stub plate that says plainly what is gone —
  "Studio Printer was unbound on <date>. Its facts are no longer read." Worlds still works.

States are words **plus a shape** (solid rule / bracketed plate / dashed rule / empty
outline), never colour alone. A visible **MOCKUP** label runs in the metadata strip, and the
colophon states that nothing on the page is a real reading. Floors held: 390 px with no
sideways scroll, 44 px targets, full keyboard reach with visible focus,
`prefers-reduced-motion`, and no restyle of the old FdApp.

## 7. Borrowed, and deliberately different

- **Homarr** — a plate is a view over another system's data; but the folio is a *document*,
  not a tile wall, and there is no drag-to-arrange.
- **Dashy** — checked-at per fact; but shown as a printed provenance **stamp**, not a dot,
  and the configuration-as-document idea is taken literally into print.
- **HA cards** — the explicit state set (`live` / `stale` / `unavailable` / `unknown`) as
  first-class words; reused directly.
- **MCP ext-apps** — sandboxed embed and host-mediated calls; but the shell *frames* the app
  and keeps its running heads, rather than the app becoming the page.

## 8. Starting point and guide rules

Atelier Zero supplies the tokens (paper `#efe7d2`, ink `#15140f`, single coral accent
`#ed6f5c`, Inter Tight / Playfair italic / JetBrains Mono, Roman rules, metadata strip,
side rails); `stitch-design-taste` supplies the method — decisions pinned before code,
one accent with saturation under 80%, asymmetric layout, no centred hero, no 3-column equal
cards, explicit anti-patterns. Applied rules: [anti-ai-slop](../../od-pack/anti-ai-slop.md)
(no emoji, no invented metrics, no filler copy), [color](../../od-pack/color.md) (neutrals
70–90%, accent ≤ 2 uses/screen, no pure black/white), [typography](../../od-pack/typography.md)
(ALL-CAPS tracking ≥ 0.06em, negative display tracking, ≤ 2 families, 65 ch body),
[laws-of-ux](../../od-pack/laws-of-ux.md) (Hick's ≤ 3–5 primary options, chunking, Von
Restorff paired with a shape), [state-coverage](../../od-pack/state-coverage.md) (five states
+ Connect test run-state + retry backoff), [accessibility-baseline](../../od-pack/accessibility-baseline.md)
(WCAG 2.2 AA, focus-visible, landmarks, labelled controls), and
[animation-discipline](../../od-pack/animation-discipline.md) (150 ms state-confirmation,
motion confirms rather than performs).

## 9. Signature element, hero thesis, reference lock

**Signature:** the **plate** — a fact rendered as a museum label: hairline corner brackets, a
Roman-numeral plate number, a coral full-stop, and a monospace provenance stamp
(`SOURCE local · CHECKED 14:23:58Z · AGE 28s`). **Hero thesis:** the hero is one fact, fully
stamped — *"A fact is an artifact; Worlds is the catalogue."* **Reference lock:** the page
furniture of **Stripe Press books** (running heads, folio numbers, margin notes) and
**museum object labels** (plate + accession number + provenance line) — mechanics only, not
palettes.

## 10. Risks and UNKNOWNs

- **Font conflict.** `stitch-design-taste` bans Inter and restricts serifs; Atelier Zero
  mandates Inter Tight / Inter / Playfair. Lever 1 binds the assigned design system, so the
  folio follows Atelier Zero and records the deviation for the owner.
- **Motion conflict.** The skill asks for perpetual micro-loops; Atelier Zero and
  `animation-discipline` say editorial pages do not animate themselves. Only the 2.4 s pulse
  dot survives, gated by `prefers-reduced-motion`, and it is non-essential.
- **No external assets.** Atelier Zero is collage-first, but the stack rule bans external
  images and fonts. The prototype has no collage imagery; the plate, brackets and type carry
  the look. The real faces (Inter Tight, Playfair Display) are **UNKNOWN** offline and render
  through system fallbacks until the build step decides.
- **Legibility at 390 px.** The metadata strip and side rails must collapse; whether the
  running heads crowd the phone width is **UNKNOWN** until built.
- **Connect lifecycle details** beyond the brief's shape are **UNKNOWN** in this session.
- This is a **new** concept, not an evolution of `patchbay` / `nowline` / `altitude`; the
  nearest round-1 relative, `patchbay`'s routing board, is deliberately not reproduced.

---
## DESIGN.md for c842

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
