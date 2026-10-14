# Worlds vNext round 2, step 2: critique three DESIGN.md files (another author's)

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- Never one-shot: every design gets a critique by a different engine before it is built. You are that critic.

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. Below are three DESIGN.md files by another author. For EACH, write `docs/vnext/round2/<concept-id>/CRITIQUE.md` with AT MOST 5 gaps, most important first. Each gap: what is missing or wrong against the brief (cite the requirement number or floor), why it matters, and the concrete fix. Focus on integration (plug-in live, composition, embedded frame, one fact many places, governed handoff, disconnect), honesty (stale/unknown, no invented data), uniqueness vs the anti-repeat list, and whether the OD skill/design system was really used. Do not rewrite the design; do not grade generously; if a design has fewer than 5 real gaps, list fewer. Write only the three CRITIQUE.md files. Do not push.


---
## DESIGN.md for c399

# c399 — The Folio

Status: DESIGN.md, written before any code. Nothing here is built yet.
Brief: [concept-brief-v2.md](../../concept-brief-v2.md) · Data:
[fixture.json](../../fixture/fixture.json) · Mechanics:
[design-research.md](../../design-research.md)

Starting point: OpenDesign skill
[after-hours-editorial-template](../../od-pack/skill-after-hours-editorial-template.md)
+ design system [ds-warm-editorial](../../od-pack/ds-warm-editorial.md).

---

## 0. Two conflicts between my starting point and the brief, resolved up front

Recorded here rather than buried, because both are real and both change what
gets built:

1. **The skill is a video template; the brief is an application.** Its own
   frontmatter says `surface: video`, `platform: desktop`, "three narrative
   pages", "keep each page under 3 seconds", "ambient grain/vignette",
   "chapter wipe transitions", "restrained cursor-light interaction". An
   operator surface whose facts go stale in front of you cannot autoplay on a
   3-second clock; it also cannot be desktop-only (the floor is 390px), and
   motion-on-by-default is on the brief's exclude list.
   **Resolution: I take the skill's craft — staged text reveal *hierarchy*,
   chapter structure, restrained typographic tone, one idea per page — and
   reject its motion contract entirely.** No autoplay, no wipes, no grain loop,
   no cursor-light, no page dwell. This is not a subversion of the skill: the
   skill's own step 1 says to map the *active* DESIGN.md into CSS variables and
   preserve the baseline, and the sibling `frontend-design` skill's OpenDesign
   section states the governing rule — "if the user's brand guidance conflicts
   with a generic craft rule, the user's brand guidance wins."
2. **The design system's own primary colour fails AA as body text.** See §8 for
   the measured numbers and the deviation I am taking, flagged for its owner.

## 1. Metaphor in one line, and the main gesture

**An evening paper assembled from wire copy that other desks filed.** Worlds does
not gather data; it *sets type*. Each service is a department with its own
editor, its own copy and its own authority, and the paper is only as honest as
its proofs.

**Main gesture: read the column, then turn the page.** The user's motion through
Worlds is a page turn, not a drill-down — the next department is a sibling
"page" of the same issue, reached by a folio rail rather than by opening an app.

Anti-repeat note: not "plain-text desk" (round 1) — this is a *composed
broadsheet* with hierarchy, a running head, marginalia and a pull-quote, not a
raw text surface. Not "time-scrubbing now-line" (round 1) — nothing scrubs;
freshness is a printed "set at" time.

## 2. Integration model

**Where a service lives.** A department on the front page (a bounded editorial
block: kicker, deck, byline-with-source, and its facts as a set list), plus an
entry in the back-of-paper **INDEX**, which is the same facts again in ledger
form.

Start state (2 connected): `media-shelf` (healthy) and `front-door`
(needs_attention). `local-weather` sits on the page as the zero-provider empty
department, with no facts and no placeholder. The third service is added
through the lifecycle: **`notes-sync`** (unavailable, external source,
human-only retry) — chosen because a sync provider that has gone quiet is the
most honest reason to show the Connect lifecycle's "safe test" failing.

**Connect lifecycle**, rendered as an **admission to the issue** — the six
brief stages, named in editorial language, with the fixture untouched. Stage
names only; the contract draft #329 is not in this repository so field names are
**UNKNOWN** (§10).

| Stage | In this concept's language | What is on screen |
|---|---|---|
| discover | *Find the desk* | the paper's "DEPARTMENTS" masthead list of candidates |
| credential reference | *File the byline* | a name for the reference, e.g. `notes-sync / local`. **No field can hold a secret**; the label says so |
| safe test | *Proof the desk* | "RUN A PROOF" with `PROOFING → PASSED / FAILED / TIMED OUT`, 6 s cap, 15 s "this is taking longer" line |
| preview of facts | *Read the copy as it stands* | the service's fixture facts with their states, plus "nothing is published yet" |
| choose what to publish | *Choose what runs* | per-fact checkboxes; unchecked facts never appear again; count of held-back facts printed |
| it appears | *It is in the paper* | the department sets into the front page, INDEX gains its rows |

**How facts flow.** A single store keyed by fact id, read by every surface. The
governed action (`frontdoor.back-door` → lock, from the fixture's
`crossViewExample`) writes once; the pull-quote, the department and the INDEX all
re-compose from it. Nothing is copied between views.

## 3. Navigation between Worlds and a service

- **Desktop (≥1024px):** a two-page spread on one screen — body 7 columns
  (departments, stacked in editorial order) and margin 5 columns (the running
  apparatus: standing order, folio, INDEX jump-list). The spread *turns* between
  three settings: **FRONT** (home), **DEPARTMENT** (one service in full), and
  **INDEX** (the ledger). A page turn is a 150ms opacity crossfade only —
  never a wipe, never a slide that pushes content the user did not ask to move.
- **Phone (390px):** one column. The spread collapses to FRONT; a DEPARTMENT is
  reached by a full-width, 44px-tall department card that is a real button. A
  sticky folio bar at the bottom carries `FRONT · INDEX` and a Back control —
  no scroll-jacking, no scroll-snap, no sideways scroll at any width.
- **Keyboard:** every card, folio control, fact row and action is a real
  control in DOM order, with a 2px terracotta focus ring and a 2px offset.
  Choosing a department moves focus to its kicker heading; the Back button
  returns focus to the card that opened it.

## 4. The embedded-app frame

- An `srcdoc` iframe carrying a small standing-in rendering of the owning app's
  own UI, set into the body as a **galley block**: crop marks at the four
  corners, a hairline rule above, and a caption line in the muted token —
  `SET BY MEMORY · OPEN IN THE MEMORY APP →`.
- Sandbox: `sandbox="allow-scripts"` so the embedded app can call back to the
  host by `postMessage`; **no** `allow-same-origin`, **no**
  `allow-top-navigation`. It cannot read the host DOM.
- **Two fallbacks, and the distinction is the point:**
  - *Owner fallback* — the fixture's `owner.fallback` label and `app://…` href,
    verbatim ("Open Memory app" → `app://memory`).
  - *Ext-app fallback* — `https://example.invalid/…` for a third-party frame,
    as the brief requires and as the fixture's `_meta.syntheticNote` permits
    (it reserves `.example.invalid` for any placeholder host added later). The
    fixture supplies no ext-app URL, so this string is prototype-only and
    marked MOCKUP.
- The shell keeps around it: the running head, the department kicker, the
  byline with source and "set at" time, the folio rail, and the back control.
- The frame scrolls **inside its own box**; the shell never scrolls with it, so
  the user never loses their place in the paper.

## 5. The agent/human shared-facts model

**Thesis: the agent reads the INDEX; the human reads the article. They are the
same paper, set from the same copy.**

- The **INDEX** (back of paper) is the agent-facing surface: one ledger row per
  fact, with the source in the **margin note** (`local` / `external` / `none`),
  the freshness on the same row as `SET 14:20:00Z · 4M AGO`, and a two-column
  permission table:
  `AGENT: read` | `HUMAN: read`, etc.
- Permissions are **two independent columns**, never one merged "access" field.
- `none` is printed, not hidden, with the fixture's reason set in italic beneath
  it — `frontdoor.guest-code-used`: *"Identifying a person is human-only in this
  fixture; the agent sees that a guest code was used, not who."*
- `act` never becomes a Worlds-side control. It prints as a hand-off line in
  the body copy: `THE DECISION IS YOURS — SET IT IN HOME →`. Worlds routes; it
  does not decide.
- Freshness is recomputed from `checkedAt` at render time; the fixture's
  `ageSeconds` is never trusted, per the fixture's own instruction.

## 6. States

All of these are written in the language of the paper, and none of them is a
blank space.

- **First run (zero services).** The paper prints its own emptiness: a masthead
  and the line `NO DEPARTMENTS ARE SET IN THIS ISSUE YET`, then the Connect
  entry point. No sample content is shown as if it were the reader's own.
- **Zero providers.** `local-weather`: `NOT CONFIGURED`, the fixture's
  `emptyReason` in full, and a single hand-off to provider settings. No dashes,
  no zeroes, no skeleton standing in for a value.
- **Quiet.** `media-shelf`: a calm deck in the foreground ink, one muted
  "set at" line, and nothing else. Nothing on the page moves when nothing
  changed.
- **Needs you.** `front-door`: the pull-quote slot is taken by the back door
  fact at display size, terracotta used exactly once as a rule beneath it, the
  fixture's `attentionReason` quoted, and the hand-off control.
- **Stale.** `nightly-backup`: values printed with the state word `STALE`
  beside each, last-successful timestamp carried through, and a "set at" line
  in foreground ink — 8h 24m spelled out, never a bare colour.
- **Unknown.** `garden-camera`: the fact slots set as `UNKNOWN`, the margin
  note reads `no source`, and the fixture's `unknownReason` is printed. An
  absent fact is never rendered as an empty space.
- **Unavailable.** `studio-printer` / `notes-sync`: cause in plain words
  (`provider timed out`), the human-only retry with its attempt count, and after
  three attempts the retry becomes `CONTACT SUPPORT` with a copyable error id.
- **Disconnected.** Disconnecting `notes-sync` leaves a single-line colophon:
  `NOTES SYNC WAS DISCONNECTED AT 14:31. ITS THREE FACTS ARE NO LONGER
  PUBLISHED HERE; NOTHING ELSE IN THIS ISSUE CHANGED.` A sentence, not a
  control.
- **Loading.** Any proof or probe carries a 15-second "this is taking longer
  than expected" line, and stops at 60 seconds with an error and a retry. No
  indefinite spinner.

## 7. Borrowed, and deliberately different

- **Borrowed from Home Assistant cards** — `unavailable` and `unknown` as
  *rendered* states rather than a stale number; and actions declared per item as
  data. *Different:* instead of a card with three gestures, each department
  carries a set list whose rows expose a primary (read) and a secondary
  (hand-off) control, both labelled in words.
- **Borrowed from Dashy** — a per-item status check that owns its own
  checked-at time. *Different:* Dashy shows reachability; this paper shows
  freshness **and** authority side by side, because a reachable system and a
  decided-with system are different questions.
- **Borrowed from Homarr 2.0** — a tile is a view over another system's data,
  not a copy; and an explicit edit mode. *Different:* this paper has no edit
  mode at all in the prototype — the order of departments is editorial and
  fixed, because a drag-to-rearrange newspaper would turn a judgement into a
  layout preference.
- **Borrowed from MCP ext-apps** — a sandboxed frame that asks the host for
  anything it wants to do. *Different:* the permission column is *printed*
  beside every fact, so the agent and the human can both read the rule
  governing it instead of it being an invisible gate.
- **Deliberately not borrowed:** Appsmith's expression-binding editor (a second
  source of truth for a mockup) and Dashy's config-as-file editing (the fixture
  is the config here and it must not be mutated).

## 8. OpenDesign starting point and the guide rules applied

**Skill `after-hours-editorial-template`** — used for: the resource discipline
(single self-contained file, inline CSS and JS, no sandbox-hostile APIs — so
**no `localStorage` and no `confirm()`**, and the disconnect flow uses an inline
confirm control instead); the hierarchy of staged text reveal, implemented as
staged *entry order* on first paint only and never replayed; the one-idea-per-
page structure; and its checklist step as the pre-ship gate. Its motion section
is rejected — see §0.1.

**Design system `ds-warm-editorial`** — taken as the brand contract:

| Token | Hex | Use |
|---|---|---|
| Background | `#FAF7F2` | paper |
| Foreground | `#1C1A17` | all body copy and UI text |
| Accent | `#C0512F` | terracotta — **use restricted, see below** |
| Accent 2 | `#2F5B4F` | section rules, department tags |
| Muted | `#8A817A` | non-essential marginalia and the galley caption only (it measures 3.57:1 — see below) |
| Surface | `#FFFFFF` | raised cards only |

Layout: 12 columns, 1200px max, 24px gutters, ≥80px section spacing at desktop,
48px tablet, 32px phone. Type: display serif at `1.2` leading with `-0.02em`
above 40px, body sans at `1.6`, scale 12·14·16·20·28·40·56·80. H1/H2 title
case, H3 and below sentence case. No pure black, no pure white in user-facing
copy. Max two elevation levels; no shadows except the 2px/16px/6% hover on
cards.

**The measured deviation, stated openly.** Against `#FAF7F2`, `#C0512F` is
**4.41:1** — it fails AA for normal-size text (4.5:1) in *both* directions: as
text on paper, and as the DS's specified "terracotta fill + off-white label"
button (same pair, 4.41:1). The DS says tokens are non-negotiable and forbids
inventing hex; the craft rules forbid shipping 4.41:1 as body text. So the
terracotta token is kept exactly and **rationed to roles that pass**:

- one 1px rule under the pull-quote (a non-text use, only contrast 3:1 is
  required — it passes);
- one eyebrow word in the running head, set at 19px/600 — the WCAG "large text"
  threshold of 18.66px bold, where 3:1 is sufficient and 4.17:1 passes;
- as the fill of a primary button whose label is set at 19px/600 for the same
  reason.

Everywhere else, small text is foreground `#1C1A17` (16.25:1). The accent-use cap
from `color.md` (≤2 visible uses per screen) is therefore satisfied by
construction. **If the DS owner rejects this, the fallback is a documented
`#A03F22`-class darkening step — but that is a new hex, which this design is
not allowed to invent, so it is raised here as a question rather than done.**

**A second measured problem, same token family.** `Muted` `#8A817A` measures
**3.57:1** on paper — also below 4.5:1. The DS assigns it to "timestamps,
metadata", which in this concept is exactly the provenance and freshness text
that the brief forbids shipping without words. **Resolution: `Muted` is confined
to non-essential marginalia and the galley caption; every provenance and
freshness line is set in foreground `#1C1A17`.** Forest `#2F5B4F` measures
7.2:1 and is used freely for section rules and department tags.

**Other guide rules applied** — `typography.md`: two faces only, ALL CAPS at
`0.08em` (the running head and folio), body ≤65ch, three weights (400/500/600),
serif only on display and figures-that-matter, no justification. `color.md`:
four-layer palette, one accent, contrast computed rather than assumed.
`anti-ai-slop.md`: no indigo, no hero gradient, no emoji, no invented metric, no
left-border-accented rounded card, no filler copy — the empty states say real
things. `state-coverage.md`: five states per surface, form states validated on
blur, retry discipline, live regions present in the DOM before injection.
`accessibility-baseline.md`: 44px targets, visible focus, contrast gates, no
colour-only status. `animation-discipline.md`: 150ms opacity/colour only for
state confirmation; no translate, no parallax, no loops — and under
`prefers-reduced-motion` the staged entry reveal is skipped entirely so the page
appears in its final state.

## 9. Signature element, hero thesis, reference lock

- **Signature element — the running head.** One line of serif small caps across
  the top that states the world's condition *in prose* and **re-composes its
  typography** when a fact changes: `FOUR DESKS FILE · ONE NEEDS YOU · ONE IS
  STALE` becomes, after the governed action, `FOUR DESKS FILE · NOTHING ASKS ·
  ONE IS STALE` — same line, different line break, because the set width
  changes. It is the one element that makes "one fact, many places" visible
  without a chart.
- **Hero thesis — the pull-quote.** The single fact that needs a human, set at
  56px in the display serif, top-biased in a 72vh page with the rest of the
  content below the fold. It is a thesis, not an illustration: *"The back door
  has been unlocked for forty-seven minutes."* If nothing needs a human, the
  slot does not fill with decoration — it prints `NOTHING NEEDS YOU TODAY` and
  the page is allowed to be short.
- **Reference lock (mechanics only; `[memory]`, not verified in this session):**
  **The New Yorker's article layout** — the running head, the deck, the
  marginal note, and the discipline of one accent per page; and **a
  letterpress/broadsheet front page** — the pull-quote as the page's one
  oversized voice and the colophon as the honest endnote. Palette and type come
  from ds-warm-editorial alone.

## 10. Risks and UNKNOWNs

- **UNKNOWN — Connect contract draft (#329).** Not in this repository (grepped
  `docs/`); only the brief's six stage names exist. Per-stage copy, field names,
  and whether the credential reference is a vault name or an opaque pointer are
  unverified.
- **The terracotta contrast deviation (§8) is unresolved.** It is compliant as
  designed, but a reviewer who reads "buttons must be 14px labels" will call it
  a regression. Needs a decision from the design-system owner, not a silent fix.
- **Fonts.** The stack rule forbids external fonts, so `'GT Sectra'` and
  `'Söhne'` will resolve to `'Times New Roman', serif` and `system-ui`. The
  editorial voice is carried mostly by the serif fallback; whether the review
  machine has the real faces is UNKNOWN. This is the largest fidelity risk in
  the concept — the design leans on typographic personality.
- **UNKNOWN — whether any real provider accepts this embed policy.** CSP
  `frame-ancestors` and `X-Frame-Options` are not verifiable here; the five
  references in design-research.md were all unverified in that session.
- **Editorial framing risks over-promising.** "Paper" language can read as
  decoration if the numbers are not doing work. Mitigation: every department
  carries its source and age in words, and there is exactly one accent per
  screen. If it still reads as a magazine skin over a dashboard, the honest fix
  is to lose the metaphor, not to add more print detail.
- **Not decided:** whether the INDEX should be a real second page on phone or a
  section at the foot of FRONT. Leaning to a second page, which costs one more
  tap on the floor-sized screen.
---
## DESIGN.md for c589

# c589 — The Instrument Desk

Status: DESIGN.md, written before any code. Nothing here is built yet.
Brief: [concept-brief-v2.md](../../concept-brief-v2.md) · Data:
[fixture.json](../../fixture/fixture.json) · Mechanics:
[design-research.md](../../design-research.md)

Starting point: OpenDesign skill
[frontend-design](../../od-pack/skill-frontend-design.md) + design system
[ds-hud](../../od-pack/ds-hud.md).

---

## 1. Metaphor in one line, and the main gesture

**A night-shift operator's instrument desk where every instrument belongs to a
machine you do not own.**

You have authority over the *wiring*, never over the *readings*. That is the
whole thesis, and it is what separates this from a cockpit HUD: in a cockpit the
panel is the aircraft's own nervous system; here each cell is a rented feed, and
the desk's job is to show honestly how fresh each feed is and to route every
decision back to the system that owns it.

**Main gesture: sweep and clear.** You read the one advisory that is coloured
and word-labelled at the top of the desk, you press the owning system's control
(the desk never performs the decision), and the fact's propagation visibly
clears the advisory out of the home cell, the service surface and the WIRE panel
at the same instant.

Anti-repeat note: HUD is on the anti-repeat list by way of "Horizon Focus HUDs".
This concept takes the ds-hud *token set and layout discipline* and rejects the
cockpit framing — no attitude/heading symbology, no ownship, no flight metaphor
for the data. See §10.

## 2. Integration model

**Where a service lives.** One cell in the instrument grid, plus two derived
surfaces. A service is never a "page" — it is one column of instruments that
Worlds composes with its neighbours.

Start state (2 connected, per the brief): `media-shelf` (healthy) and
`front-door` (needs_attention). `local-weather` is present on the desk as a
`not_configured` cell with zero facts and an empty state (the zero-provider
floor). `nightly-backup`, `garden-camera`, `studio-printer` and `notes-sync` are
offered by the ADD drawer, unconnected, using their fixture ids and states
verbatim.

**Connect lifecycle** (stage names from the brief; the contract draft #329 is
not in this repository, so field names and per-stage copy are **UNKNOWN** —
see §10):

1. **DISCOVER** — pick from the ADD drawer; the candidate's own summary is shown
   from the fixture, not invented.
2. **CREDENTIAL REFERENCE** — the form takes a *reference name only*, e.g.
   `front-door / local`. It never accepts a secret value; there is no field that
   could hold one, and the label says so in words.
3. **SAFE TEST** — an explicit "run test" button showing `RUNNING → PASSED` or
   `FAILED` in place (state-coverage's explicit-run-state pattern). No spinner
   without a timeout; the desk's test has a 6 s cap and then says
   `TEST TIMED OUT — no answer from the owning app`.
4. **PREVIEW OF FACTS/CAPABILITIES** — the fixture's facts for that service are
   listed with their own state, exactly as they will appear, with a "nothing is
   published yet" line.
5. **CHOOSE WHAT TO PUBLISH** — per-fact checkboxes. A fact left unchecked is
   never rendered afterwards, and the count of unpublished facts is stated
   ("2 facts held back").
6. **IT APPEARS** — the cell is inserted into the grid and the WIRE panel gains
   its rows, from one state change.

**How facts flow.** One in-memory store keyed by fact id, derived from the
fixture; every surface is a pure read of that store. No surface owns a copy. The
governed action (`frontdoor.back-door` → lock, using the fixture's
`crossViewExample`) writes to the store, and all three surfaces re-render from
it. The fixture is not mutated on disk.

## 3. Navigation between Worlds and a service

- **Desktop (≥1024px):** three fixed regions — a left **CELL RAIL** (service
  list, one line each, uppercase micro-label + mono state word), the **DESK**
  (the selected service's instruments, 12-col HUD grid), and a right **WIRE**
  panel (facts, source, freshness, permissions). Selecting a rail entry changes
  the desk in place; nothing navigates away.
- **Phone (390px):** one column, in this order — advisory line, cell rail as a
  horizontally *wrapping* chip row (wraps, never scrolls sideways), desk
  instruments stacked, then WIRE as the last section. Back is a real button in a
  44px-tall sticky bar, not a gesture.
- **Keyboard:** rail entries, instruments, panel rows and every action are in
  DOM order and reachable by Tab with a 2px `#00FF41` focus outline and a
  `:focus-visible` offset. Focus moves to the desk heading when a rail entry is
  chosen with Enter (state-coverage focus rule). No pointer-only affordance
  exists.

## 4. The embedded-app frame

- A `sandbox="allow-scripts"` `srcdoc` iframe standing in for the owning app's
  own UI, drawn inside a bevelled bay with a corner label `EXTERNAL APP ·
  SANDBOXED`. Scripts are allowed so the embedded app can answer the host via
  `postMessage`; `allow-same-origin` and `allow-top-navigation` are **not**
  granted, so it cannot reach the host's DOM.
- **Two fallbacks, and the distinction is the point:**
  - *Owner fallback* — the fixture's `owner.fallback` label and `app://…` href,
    verbatim (e.g. "Open Home app" → `app://home`).
  - *Ext-app fallback* — `https://example.invalid/…` for the third-party frame,
    per the brief and the fixture's `_meta.syntheticNote`, which reserves
    `.example.invalid` for any placeholder host added later. The fixture holds
    no ext-app URL, so this one string is prototype-only and marked MOCKUP.
- The shell keeps around the frame: the desk's nav, the back control, the
  advisory line, the source/freshness caption above the bay, and a
  `LAST CHECKED` clock that keeps ticking while the frame is idle.
- The frame has a visible **offline state** of its own: a deliberate
  `SANDBOX BLOCKED` rendering with the owner link still reachable, so "the app
  could not load" is never confused with "the service has no data".

## 5. The agent/human shared-facts model

One table, one row per fact, in the WIRE panel. Columns:

| Source | Freshness | Agent | Human |
|---|---|---|---|
| `LOCAL` / `EXTERNAL` / `NONE` | `SET 14:23:58Z · 28s AGO` | `READ` / `ACT` / `NONE` | `READ` / `WRITE` / `ACT` / `NONE` |

- Source and freshness are **read from `checkedAt` at render time**, never from
  the fixture's `ageSeconds` (the fixture explicitly says a viewer should
  recompute).
- Agent and human permissions are **two independent columns**, never one
  "access" field.
- `NONE` renders as a **named, disabled control with its reason in words**, not
  hidden: `frontdoor.guest-code-used` shows `AGENT: NONE — Identifying a person
  is human-only in this fixture; the agent sees that a guest code was used, not
  who.` and `camera.human-detected-today` shows `AGENT: NONE — The agent may
  not assert a detection while the source is unknown.`
- An `ACT` permission never renders as a Worlds-side button. It renders as a
  hand-off: `DECIDE IN HOME →` pointing at the fixture's owner route. Worlds
  never decides; it routes.

## 6. States

Every surface renders all of these; none is left implied.

- **First run / zero services.** A 3-line origin block (`WORLSDESK / NO CELLS
  WIRED / Add a service from the drawer to begin`) plus the Connect lifecycle
  entry point. No demo data is shown as if it were the user's.
- **Zero providers.** `local-weather` cell: `NOT CONFIGURED` in words, plus the
  fixture's `emptyReason`, plus one hand-off link to provider settings. No
  placeholder value, no dashes, no skeleton pretending to be a fact.
- **Quiet.** `media-shelf` healthy: a low-contrast `STANDBY` treatment, values
  in `--data-secondary`, and nothing animated. Nothing moves when nothing
  changed (ds-hud: do not animate what does not signal operational state).
- **Needs you.** `front-door`: `NEEDS A HUMAN` in `--data-warning`, the
  attention reason from the fixture quoted in words, and the hand-off control.
- **Stale.** `nightly-backup`: value shown in `--data-secondary` with the state
  word `STALE`, the last-successful timestamp, and `LAST CHECKED 06:00:00Z ·
  8H 24M AGO` — age always spelled, never a bare colour.
- **Unknown.** `garden-camera`: the fact slots render as `UNKNOWN — no source
  has answered` and `NONE` appears as the source. A blank is never a state.
- **Unavailable.** `studio-printer` / `notes-sync`: cause in words
  (`job rejected: no media loaded`, `provider timed out`) plus the human-only
  retry with its attempt count, following state-coverage's retry discipline
  (immediate first retry, "last attempted Xs ago", disabled with a reason after
  three).
- **Disconnected.** Disconnecting `garden-camera` leaves a tombstone cell for
  one page: `GARDEN CAMERA — DISCONNECTED 14:31. Its three facts are no longer
  published here and nothing else changed.` The tombstone is a statement, not a
  trash icon.
- **Loading.** Every probe has a 15 s "taking longer than expected" line and a
  stop-after-60 s error, never an indefinite spinner.

## 7. Borrowed, and deliberately different

- **Borrowed from Homarr 2.0** — a tile as a live *view* over an integration
  that owns its own data; and an explicit edit mode for arrangement so the live
  view cannot be changed by accident. *Different:* the desk has no free
  arrangement at all — cell order is by urgency then name, fixed. Re-arranging a
  panel you do not own would be a lie about what matters.
- **Borrowed from Dashy** — a visible per-item status check that carries its own
  checked-at time, and configuration as a reviewable document. *Different:* the
  `NOT CONNECTED` drawer is generated from the fixture and is not editable in
  the prototype (see §10).
- **Borrowed from Home Assistant cards** — states are first-class and include
  `unavailable` and `unknown` as *rendered* states rather than a stale number;
  and actions declared as data per item (here: view / hand off / retry).
  *Different:* HA treats tap/hold as three gestures; the desk uses three
  explicit, labelled, focusable controls instead, because hold is not
  discoverable and not keyboard-reachable.
- **Borrowed from MCP ext-apps** — a sandboxed frame that talks back through
  the host, never directly to a service; the host checks permission before
  dispatching. *Different:* the desk's host-mediated call is *shown* — the
  permission column is visible in the same row as the value — rather than being
  an invisible check.
- **Deliberately not borrowed:** Appsmith's drag-to-arrange editing and its
  expression-binding editor (too much machinery for a mockup, and it would make
  the config a second source of truth).

## 8. OpenDesign starting point and the guide rules applied

**Skill `frontend-design`** — used for: step 2's "commit to one specific
aesthetic direction" (dense operational, night-light only, no light mode);
step 3's "design the real interface, not a placeholder poster" (controls,
filters, empty/error states and the full Connect form are all in scope);
step 5's density rule (this is a repeated-use tool, so it is dense, not airy);
step 6's self-review, run against the floors in §6. Its step-2 banned list is
the same as anti-ai-slop and is honoured in full.

**Design system `ds-hud`** — taken as the brand contract, no values invented
outside it:

| Token | Hex | Use here |
|---|---|---|
| `--bg` | `#0A0A0A` | desk canvas |
| `--surface` | `#111316` | cell and panel fills |
| `--border` | `#1E2328` | panel separation only |
| `--data-primary` | `#00FF41` | live values, focus ring |
| `--data-secondary` | `#7FFF00` | standby / stale values |
| `--data-tertiary` | `#5A9A5A` | grid lines, tick marks, **labels only** |
| `--data-warning` | `#FFB800` | needs-a-human advisory |
| `--data-alert` | `#FF3B3B` | unavailable / fault |

Contrast gates computed against `#0A0A0A` rather than assumed: primary 14.5:1,
secondary 15.28:1, tertiary 5.85:1, warning 11.42:1, alert 5.6:1 — all pass AA, so
tertiary is safe for the 10–12px uppercase labels (ds-hud forbids tertiary for
*readout* text; that is obeyed — it carries labels and rules, never a value).
Dark only, no light mode, flat fills with no gradients, corners 0 or 4px except
the circular reticle (ds-hud's 50% rule).

**Guide rules applied** — `color.md`: four-layer palette, one accent (there is
no accent; primary is a *data* colour), accent-use cap honoured by having zero
decorative accent. `typography.md`: exactly two faces, mono for all readouts,
sans for uppercase labels, ALL CAPS at `0.08em` tracking, three weights
(400/550/700), body ≤65ch, no `text-align: justify`. `anti-ai-slop.md`: none of
the seven sins — no indigo, no gradient hero, no emoji, no invented metric, no
left-border-accented rounded card, serif/display face is the mono itself.
`state-coverage.md`: all five states per surface plus the retry and timeout
discipline. `accessibility-baseline.md`: 44px minimum targets, visible focus,
no colour-only status. `animation-discipline.md`: 150ms state confirmation,
100ms linear for value changes, transforms/opacity only, and under
`prefers-reduced-motion` every transition is stripped to an instant state change
— **the propagation tick does not run; the same fact is instead marked with a
static 2px outline for 2s, which conveys the same information without motion.**

## 9. Signature element, hero thesis, reference lock

- **Signature element — the propagation tick.** When one fact changes, a 1px
  `#00FF41` rule sweeps each affected readout in the same frame, so the user
  sees *one event, three places* rather than three coincidences. Under reduced
  motion it becomes the static outline described above. One element, used once
  per event, no ambient loops.
- **Hero thesis — the advisory line.** The single highest-urgency fact, set at
  32px mono, top-anchored above the desk, with its age spelled in words beneath
  it. The hero is a thesis, not a picture: *"Front door: back door unlocked for
  47 minutes. A decision is required; the local app will not act on its own."*
- **Reference lock (mechanics only; `[memory]`, not verified in this session):**
  the **Garmin G1000 flight deck** — a persistent advisory that takes over the
  primary field and does not repeat elsewhere — and the **layout of a military
  C2 / ISR operator station** — many feeds, equal visual weight, the operator's
  authority deliberately absent from the feeds. Palette and glyphography are
  taken from ds-hud only; nothing is copied from either.

## 10. Risks and UNKNOWNs

- **UNKNOWN — Connect contract draft (#329).** Not present in this repository
  (grepped: no connect-lifecycle or credential-reference contract under
  `docs/`). Only the six stage names from the brief are used. Real field names,
  per-stage copy, and whether the credential reference is a named secret in a
  vault or an opaque pointer are all unverified.
- **UNKNOWN — whether any real provider would accept this embed policy.** The
  `sandbox="allow-scripts"` + `srcdoc` frame proves the mechanics, but real
  providers gate embedding with CSP `frame-ancestors` and `X-Frame-Options`, and
  none of the five references in design-research.md was verified in this
  session. The MCP ext-apps spec was itself moving fast when that note was
  written.
- **Fonts.** The stack rule forbids external fonts, so `JetBrains Mono` and
  `Inter` will usually resolve to their local fallbacks. The mono fallback
  carries the identity; the sans fallback for 10–12px uppercase labels is a
  weaker match. Whether the review machine has either face is UNKNOWN.
- **HUD vs the anti-repeat list.** "Horizon Focus HUDs" is on it. The bet is
  that an operator desk with no ownship, no heading/attitude symbology and an
  explicit no-authority thesis is a different artifact. A reviewer who disagrees
  can reject the whole concept — that is the intended consequence of an assigned
  design system.
- **`ageSeconds` freshness.** Recomputing from `checkedAt` means the desk's ages
  drift upward the longer the file sits open. That is correct behaviour but it
  will make screenshots taken minutes apart disagree. Unknown whether the
  review wants a frozen clock.
- **Not decided:** whether the cell rail should show unconnected services at all
  after a disconnect, or only during Connect. Leaning toward hiding them, which
  would reduce the empty/drawer distinction.
---
## DESIGN.md for c736

# c736 — The Plate

Status: DESIGN.md, written before any code. Nothing here is built yet.
Brief: [concept-brief-v2.md](../../concept-brief-v2.md) · Data:
[fixture.json](../../fixture/fixture.json) · Mechanics:
[design-research.md](../../design-research.md)

Starting point: OpenDesign skill
[algorithmic-art](../../od-pack/skill-algorithmic-art.md) + design system
[ds-dithered](../../od-pack/ds-dithered.md).

---

## 0. Two conflicts between my starting point and the brief, resolved up front

1. **The skill is a p5.js skill; p5.js is a CDN.** The brief's stack rule is a
   single self-contained HTML file with **no frameworks, no CDN, no external
   fonts or images**. p5.js cannot be loaded. The skill's actual contribution is
   not the library — it is **seeded randomness so every render is
   reproducible**. That contribution is reimplemented here in ~60 lines of
   hand-written vanilla JS: an FNV-1a hash of `factId|state` seeds an 8×8 Bayer
   ordered-dither threshold grid drawn on a `<canvas>`. Same seed, same pixels,
   every time, in any browser, with no dependency. This is a better fit for an
   integration shell anyway: a plate that renders differently on two machines is
   a bug, not an aesthetic.
2. **The design system's Secondary is on the anti-ai-slop banned list.**
   `ds-dithered` §2 sets Secondary to `#8B5CF6`, which is one of the seven hexes
   `anti-ai-slop.md` rule 1 names as the Tailwind-indigo tell (and which the
   brief's exclude list bans outright). The design system also says "do not
   introduce off-palette colors", so I am not inventing a replacement either.
   **Resolution: `#8B5CF6` is demoted out of every accent and UI role.** It
   appears only as ink inside the generated dither field — a texture, never a
   fill, never a label, never a border on a control. `--accent` is the system's
   own Primary `#3B82F6`. Recorded as a question for the DS owner in §10.

The `dithered` design system is also a *starter*: eight tokens, no decision
about dark mode, and no definition of the dot-pattern technique its own title
names. The technique is therefore mine to specify (§4, §8), inside its tokens.

## 1. Metaphor in one line, and the main gesture

**A directory printed from letterpress plates: each service is one plate, and you
proof a plate before you print it.**

The metaphor's whole claim is about *authority*. A plate belongs to the desk that
cut it. Worlds holds the sheet, runs the proofs, decides what gets printed, and
can cut a plate loose — but it never re-cuts one.

**Main gesture: proof, then print.** You proof a candidate plate (see its facts
as they would print), choose what runs, then press it into the sheet; later, when
a plate goes stale or a service disappears, you *lift* the plate and the sheet
says in plain words what left it.

Anti-repeat note: unlike the other two concepts in this round, there is no
priority order anywhere in this design. Every connected service occupies an
identical cell on an identical grid. Nothing is larger because it is urgent —
urgency is carried by a word and a dot density, never by size.

## 2. Integration model

**Where a service lives.** One cell in the plate sheet — an identical
8px-module tile containing the service's rendered plate, its name, its state
word, and its fact count. Plus one row per fact in the **LEDGER** (the tabular
view beneath the sheet). A service is never a page; it is a plate you can look
at, prove, and lift.

Start state (2 connected): `garden-camera` (unknown — so the sheet opens with an
honest hole in it, not a clean-looking grid) and `media-shelf` (healthy).
`local-weather` is on the sheet as `not_configured` with zero facts and an empty
cell. `front-door`, `nightly-backup`, `studio-printer` and `notes-sync` are
available unconnected in the ADD drawer, with fixture ids and states verbatim.
The third service is added through the lifecycle: **`studio-printer`**
(unavailable, human-only retry) — the clearest case where a proof fails and the
sheet must not pretend otherwise.

**Connect lifecycle** — the six brief stages. Field names and per-stage copy
are **UNKNOWN**; the contract draft (#329) is not in this repository (§10).

| Stage | On the plate sheet |
|---|---|
| discover | ADD drawer: candidate cells rendered in the unconnected (outline-only) plate style |
| credential reference | a reference-name field, e.g. `studio-printer / local`. **No field exists that could hold a secret**; the caption says so in words |
| safe test | `RUN PROOF` with `PROOFING → PASSED / FAILED / TIMED OUT`, 6 s cap, 15 s "taking longer than expected", 60 s stop-with-error |
| preview of facts | the fixture's facts rendered exactly as they will print, with their own states, plus "nothing is published yet" |
| choose what to publish | per-fact checkboxes; unchecked facts never render again; the count held back is printed |
| it appears | the cell prints into the sheet, the LEDGER gains its rows |

**How facts flow.** One store keyed by fact id; every cell and every ledger row
is a pure read. The governed action (`frontdoor.back-door` → lock, from the
fixture's `crossViewExample`) writes once and all three surfaces re-render. The
fixture file on disk is never mutated.

## 3. Navigation between Worlds and a service

- **Desktop (≥1024px):** a **contact sheet** — a fixed 8px module grid, 4 columns
  at ≥1200px, 3 at 1024–1199px, with the LEDGER as a full-width table beneath
  it. Selecting a cell does not navigate; it opens a **proof sheet** — a
  side panel (desktop) or a full screen (phone) — while the sheet stays visible
  behind it, so the user never loses their place among the other plates.
- **Phone (390px):** one column, in order — sheet, then ADD, then LEDGER. The
  proof sheet is a full screen with a sticky `CLOSE` (44px) that returns focus
  to the cell that opened it. No sideways scroll at any width; the cell grid
  drops to 1 column rather than becoming a scroller.
- **Keyboard:** cells are buttons with `aria-pressed`-style selection, in DOM
  order; the LEDGER rows are reachable and each row's `PROOF` control opens the
  same proof sheet as its cell. Focus ring is a 2px `#3B82F6` outline with a
  2px offset, plus a 1px white inner ring so it stays visible on a dark plate
  pixel as well as a light one.

## 4. The embedded-app frame

- An `srcdoc` iframe carrying a small standing-in rendering of the owning app's
  own UI, set into the proof sheet as an inset **proof block** with a dashed
  1px `Neutral` border and a caption strip: `EXTERNAL APP · SANDBOXED ·
  OWNED BY STUDIO`.
- Sandbox: `sandbox="allow-scripts"` so the embedded app can call back to the
  host by `postMessage`; **no** `allow-same-origin`, **no**
  `allow-top-navigation`.
- **Two fallbacks, and the distinction is the point:**
  - *Owner fallback* — the fixture's `owner.fallback` label and `app://…` href,
    verbatim ("Open Studio printers" → `app://studio/printers`).
  - *Ext-app fallback* — `https://example.invalid/…` for a third-party frame,
    per the brief and the fixture's `_meta.syntheticNote`, which reserves
    `.example.invalid` for any placeholder host added later. The fixture holds
    no ext-app URL, so this string is prototype-only and marked MOCKUP.
- The shell keeps around it: the proof sheet's header, the service's state word,
  the `SEED` chip and `RE-PROOF` control, the fallback link, and the LEDGER row
  for that service.
- The frame scrolls inside its own box. Its **own failure mode is rendered too**:
  a `SANDBOX BLOCKED` state showing the owner link, so "the app would not load"
  is never mistaken for "the service has no data".

## 5. The agent/human shared-facts model

One row per fact in the LEDGER. The generative plate carries the *state*; the
ledger carries the *rules*.

| Column | Content |
|---|---|
| Fact | label + `fact.id` in `IBM Plex Mono` |
| Plate | the seeded dither cell for this fact (see §8) |
| Source | `local` / `external` / `none` |
| Set at | `checkedAt` recomputed at render time into words (`28s ago`, `8h 24m ago`) — the fixture's `ageSeconds` is never trusted |
| Agent | `read` / `act` / `none` |
| Human | `read` / `write` / `act` / `none` |

- Agent and human are **two independent columns**, never one merged access field.
- `none` is **shown, not hidden**, as a disabled control plus the fixture's reason
  in full: `camera.human-detected-today` → *"The agent may not assert a detection
  while the source is unknown."*; `frontdoor.guest-code-used` → *"Identifying a
  person is human-only in this fixture…"*.
- `act` never renders as a Worlds-side button. It renders as a hand-off: a
  disabled-by-design `DECIDE IN HOME →` pointing at the fixture's owner route,
  with the word `HAND-OFF` printed next to it. Worlds never decides.

## 6. States

The plate system makes this concept's worst temptation *density as state*. The
rule that prevents it: **the dither is a redundant echo of a word, never the
primary carrier.** Every cell, every ledger row and every plate ships a literal
state label.

- **First run (zero services).** The sheet prints itself empty: an 8×8 grid of
  outline-only uncut cells, the line `SHEET IS BLANK — NO PLATES ARE SET`, and
  the Connect entry point. No sample data is shown as if it were the user's.
- **Zero providers.** `local-weather`: `NOT CONFIGURED` in words plus the
  fixture's `emptyReason` in full, one hand-off to provider settings, and a
  **blank plate cell with a dotted baseline** — a blank plate is legible as
  blank, which is exactly the point, and the word beside it removes all
  ambiguity.
- **Quiet.** `media-shelf`: full-density plate, state word `healthy`, `28s ago`.
  Nothing animates, ever (see §9).
- **Needs you.** `front-door`: `needs a human` in words, `Warning` dot at the
  cell corner, the fixture's `attentionReason`, and the hand-off control.
- **Stale.** `nightly-backup`: half-density plate, state word `stale` beside
  every value, last-successful timestamp carried through, `8h 24m ago` spelled
  out.
- **Unknown.** `garden-camera`: the plate renders as an **open hatch with no
  fill** (visually the absence of ink, never a grey stand-in), `source: none`,
  and the fixture's `unknownReason`. A blank is never a state.
- **Unavailable.** `studio-printer` / `notes-sync`: quarter-density plate, the
  state word `unavailable`, the cause in plain words (`job rejected: no media
  loaded`), and the human-only retry with its attempt count — immediate first
  retry, `last attempted Xs ago` after it, and `CONTACT SUPPORT` with a
  copyable error id after three attempts.
- **Disconnected.** Lifting `garden-camera` leaves a tombstone cell in place:
  `GARDEN CAMERA — LIFTED 14:31 · 3 facts no longer published here · nothing
  else changed`. The tombstone is a sentence with a date, not a delete icon.
- **Loading.** Any proof or probe has a 15-second "this is taking longer than
  expected" line, an explicit timeout at 6 s, and a stop-with-error at 60 s. No
  indefinite spinner.

## 7. Borrowed, and deliberately different

- **Borrowed from Home Assistant cards** — entity states as first-class and
  *rendered*, including `unavailable` and `unknown`, rather than showing a stale
  number; and actions declared as data per item. *Different:* the plate makes
  every cell visually identical, so a screenshot of the sheet can never be
  mistaken for a screenshot of a prioritised dashboard. State is read from the
  word, never from the composition.
- **Borrowed from Appsmith** — the reactive graph: a widget's value is an
  expression over the query result, so one write updates every dependent view;
  and named queries with visible running/error state, which is exactly what
  `RUN PROOF` is. *Different:* no expression editor is exposed. The binding is
  shown as an inspector line (`plate = seed(fact.id, fact.state)`) rather than
  as something the user can edit.
- **Borrowed from MCP ext-apps** — sandboxed frame, host-mediated calls,
  permission checked by the host. *Different:* the host's decision is printed in
  the ledger, so the permission model is legible rather than implicit.
- **Borrowed from Dashy** — per-item reachability carrying its own checked-at
  time; configuration as a reviewable document. *Different:* the config is the
  fixture and is read-only here, so the sheet cannot be reshuffled away from
  the shared baseline every concept must use.
- **Deliberately not borrowed:** Homarr's edit mode (no rearrangement, for the
  reason in §1) and Appsmith's visual editor.

## 8. OpenDesign starting point and the guide rules applied

**Skill `algorithmic-art`** — the skill's stated purpose is *seeded randomness
so every render is reproducible*, and that is the whole of what I take from it.
The plate generator: FNV-1a over `factId|state` → seed; an 8×8 Bayer threshold
matrix; a cell filled by comparing a deterministic per-pixel value against the
matrix, so the "shade" is a real ordered dither rather than a picture of one.
Density by state — `live` 1.0, `stale` 0.5, `unavailable` 0.25, `unknown`
hollow hatch (no fill), `not_configured` blank with a dotted baseline. The
`SEED` chip is displayed and the `RE-PROOF` control re-derives the identical
pattern on demand — determinism you can *see*, which is the skill's thesis
applied to integration state instead of to a poster.

**Design system `ds-dithered`** — tokens taken as-is:

| Token | Hex | Use |
|---|---|---|
| Surface | `#FFFFFF` | sheet background, cell fills |
| Text | `#111827` | all text |
| Primary | `#3B82F6` | `--accent`: primary action, focus ring, `live` dot |
| Success | `#16A34A` | healthy dot |
| Warning | `#D97706` | needs-a-human dot |
| Danger | `#DC2626` | unavailable dot |
| Secondary | `#8B5CF6` | **dither ink only** — see §0.2 |

Typography: `Space Grotesk` for display, `Open Sans` for body, `IBM Plex Mono`
for the seed, ids and timestamps; scale 14/16/18/24/32/40; spacing 4/8/12/16/24/32;
headings carry the personality, body text optimises scanning. Motion: short
transitions at 150–250ms with Primary as the interaction signal — and in this
concept the only motion that exists at all is the 150ms opacity fade when a cell
appears or is lifted. **There is no looping, generative or ambient motion**, so
`prefers-reduced-motion` removes nothing meaningful: the plate is a static
render, which is a property of the seeded approach rather than a concession.

**Contrast gates, computed not assumed** (against `#FFFFFF`): Primary 3.68:1,
Secondary 4.23:1, Success 3.30:1, Warning 3.19:1, Danger 4.83:1, Text 17.74:1.
Only Text and Danger clear 4.5:1. So the
rule for this design is: **these four colours are fills, rules, dot marks and
≥24px labels — where 3:1 applies — and every state is simultaneously spelled in
words in `#111827` (17.7:1) at body size.** That is the accessibility floor and
the palette's own constraint arriving at the same place.

**Other guide rules applied** — `anti-ai-slop.md`: the seven sins, with rule 1
handled explicitly in §0.2 rather than ignored; no left-border-accented rounded
cards (cells are 8px-radius square-cornered modules with a full-perimeter
hairline); no invented metrics; no filler copy. `typography.md`: three families
because the DS mandates them (a documented deviation from the "max two faces"
guideline, taken because the design system is the brand contract), three
weights, ALL CAPS at `0.08em`, no justification. `color.md`: off-palette values
rejected rather than invented. `state-coverage.md`: five states per surface,
loading thresholds honoured, live regions mounted before injection.
`accessibility-baseline.md`: 44px targets, focus visible on light *and* dark
pixels, no colour-only status. `animation-discipline.md`: 150ms confirmation
only, opacity only, reduced-motion honoured by construction.

## 9. Signature element, hero thesis, reference lock

- **Signature element — the visible seed.** Every plate prints the seed string it
  was rendered from, and `RE-PROOF` redraws it identically. Anyone who doubts
  whether the pattern means anything can press it twice and see it not change;
  anyone who changes a fact's state can see the plate change *because* the seed
  changed. It is the one element in this round that makes the system's own
  reproducibility the interface. Under reduced motion it is unchanged, because
  nothing here moves.
- **Hero thesis — the whole sheet at once.** The hero is not a magnified fact;
  it is the complete contact sheet, every cell the same size, with the one
  ugly truth in it — `garden-camera`'s un-inked plate — sitting in the grid
  exactly where it belongs. The thesis: *an integration you cannot see is not
  integrated.* This is deliberately the opposite composition to the other two
  concepts in this round, and that difference is the reason to keep all three.
- **Reference lock (mechanics only; `[memory]`, not verified in this session):**
  **a letterpress halftone proof sheet** — the fixed module grid, crop-marked
  blocks, the visible seed/plate and the discipline of one proof before a print
  run; and **Playdate's 1-bit dithered interface (Panic, 2019)** — the use of a
  one-bit dot matrix to carry density without gradients, and the rule that the
  dots never carry meaning a word cannot also carry. Palette and type come from
  `ds-dithered` alone.

## 10. Risks and UNKNOWNs

- **UNKNOWN — Connect contract draft (#329).** Not present in this repository
  (grepped `docs/` for connect-lifecycle and credential-reference terms). Only
  the brief's six stage names are used; field names and per-stage copy are
  unverified.
- **OPEN QUESTION for the `dithered` DS owner — `#8B5CF6`.** It is the system's
  Secondary *and* a hex that `anti-ai-slop.md` rule 1 and the brief's exclude
  list both ban. I have confined it to dither ink. If the owner wants it out
  entirely, the ink needs a different token, which this design cannot invent.
- **The DS gives no contrast guarantee.** Three of four semantic colours fail
  AA at body size (§8). The design compensates with mandatory word labels, but
  the DS itself would fail a bare-contrast audit.
- **Dither legibility at small sizes.** A 4×4 Bayer matrix in a ~28px cell may
  moiré on a 1× display or under browser zoom. Mitigation: cells are drawn on a
  canvas at `devicePixelRatio` and re-drawn on resize; the honest answer is that
  this needs a real screenshot check on a real phone, which this step cannot do.
- **Canvas and screen readers.** A dithered canvas carries no accessible
  meaning. Every plate therefore has a `role="img"` with an `aria-label` naming
  the service and state in words, and every ledger row repeats the same words in
  text. If that pairing is ever dropped, the plate becomes an inaccessible
  decoration.
- **Fonts.** No external fonts allowed, so `Space Grotesk`, `Open Sans` and
  `IBM Plex Mono` will resolve to local fallbacks. Whether the review machine has
  any of them is UNKNOWN; the mono fallback matters most here because it carries
  the seed and the ids.
- **Not decided:** whether the LEDGER should be a real table with sortable
  columns or a fixed-order list. A sortable table on 390px is hostile; leaning
  to fixed order grouped by service, which is less useful and more honest.