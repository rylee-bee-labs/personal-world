# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c572

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): stitch-design-taste atelier-zero — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c572/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c572/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

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
## CRITIQUE.md (by another engine)

# Critique — c572

1. **The folio/index-sheet concept is too close to banned prior concepts.** It explicitly cites round-one `patchbay` and leans on a catalogue/document surface, plate numbers, running folios, and provenance lines; the overall interaction is another information-routing/document surface rather than a demonstrably new shell. **Fix:** change the organizing metaphor and main gesture, not only the print styling, and compare the interaction against the full anti-repeat list.
2. **The initial service composition and live add are underspecified/inconsistent.** Media Shelf and Front Door arrive bound, while Studio Printer is “commissioned live,” but the lifecycle does not say that the person discovers it, enters a reference, runs a safe test, previews offers, chooses publication, and only then sees it appear. Studio Printer is fixture `unavailable`; the design must keep that state distinct from any connection-test result. **Fix:** specify each lifecycle step and preserve fixture IDs, names, and states verbatim throughout.
3. **The shared-facts example invents a disconnected state transition.** It claims locking `frontdoor.back-door` produces `front-door: healthy` and `attentionCount: 0`; fixture `crossViewExample` supports that outcome as an illustrative interaction, but the brief says do not mutate fixture values and instead demonstrate a change (for example, need appears or goes stale) across home, service surface, and agent view. The design names “home, plate, Index” only generally and does not explain the agent-facts view updating at once. **Fix:** make the illustrative state a clearly labeled mock interaction, leave baseline fixture values intact, and explicitly bind the same changed fact to all three required surfaces.
4. **“One link away” is not a clear embedded-app frame contract.** The app is described as a UI rendered inside a plate, but no explicit frame boundary/host navigation behavior is defined; the fixture route is Studio Printer, yet the shell link label is generic. **Fix:** name the owning app and exact reserved `.example.invalid` fallback URL, state how Worlds navigation/back/status remain available, and specify that the embedded app cannot alter the Index and hands actions through its owner.
5. **Design-system and OpenDesign conflicts remain unresolved.** The assigned Atelier Zero tokens name Inter Tight / Playfair, while the design reports unavailable faces and that the skill bans Inter; merely recording the conflict does not demonstrate the DS was applied faithfully. Also, the 2.4-second pulse survives despite the brief excluding motion on by default and Atelier Zero’s static editorial direction. **Fix:** resolve typography against the stack and system, remove default pulse motion, and identify which guide rules govern the actual built treatment.
