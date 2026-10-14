# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c399

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): after-hours-editorial-template warm-editorial — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c399/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c399/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

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
## CRITIQUE.md (by another engine)

# Critique of c399 — The Folio (DESIGN.md, not code)

Critic: a different engine from the author. Four gaps, most important first. I checked the contrast figures in §8 (4.41, 3.57, 7.2, 16.25) and they are correct.

## 1. The running head contradicts the start state and the fixture (req 2, 4; fixture "no invented data")
- **What is wrong.** §9 prints `FOUR DESKS FILE · ONE NEEDS YOU · ONE IS STALE`. Per §2 the start state is `media-shelf` and `front-door`, with `notes-sync` added later. `nightly-backup` is never connected. "Four desks" and "one is stale" are therefore false at start, and nothing in the design says where the counts come from. After the lock the head reads `NOTHING ASKS · ONE IS STALE`, which is still wrong. At two or three connected services nothing is stale.
- **Why it matters.** This is the signature element and the visible proof of "one fact, many places" (req 4). It is a hand-typed sentence that the data would contradict, which is an invented metric.
- **Fix.** Derive the head from the store as counts of connected services by state. At start it reads, for example, two desks, one needs you, none stale. After connecting `notes-sync` it adds one unavailable. Keep `local-weather` out of the "desks file" count and say so. Specify the pluralisation and the zero cases ("NOTHING ASKS").

## 2. The agent-facts view is a separate page, so "at once" is not shown (req 4); the governed action's actor is undefined (req 5)
- **What is wrong.** The INDEX is a third spread setting. On phone it is a separate page, and the design is undecided on that (§10). A change therefore updates FRONT, DEPARTMENT and INDEX, but the user can see only one of them at a time. The desktop margin holds only an "INDEX jump-list", not the ledger rows.
- **What is also wrong.** §2 says the governed action "writes once". §5 says Worlds "does not decide". Nothing says who performs the lock. If it is a Worlds button, that breaks req 5. If it happens in the owning app, the design must say the embedded frame posts a message and Worlds only reflects it.
- **Fix.**
  - Put the front-door fact rows (value, source, freshness, agent column, human column) in the desktop margin permanently.
  - On phone, show a one-line live "facts changed" summary in the sticky folio bar linking to INDEX.
  - Make the lock occur inside the embedded Home frame. The frame sends `postMessage`, the host validates the sender, and the store updates. Worlds' only control is the `app://home` hand-off link.

## 3. The hero states a time-sensitive fact as current, and the "today" claim is unsupported (honesty floor)
- **What is wrong.** The pull-quote is fixed copy: *"The back door has been unlocked for forty-seven minutes."* The fixture value is 47 minutes as of `checkedAt` 14:24:02Z. After minutes pass, that sentence is false. `NOTHING NEEDS YOU TODAY` also claims more than the shell knows, because it knows only the connected services' states at the last check.
- **Fix.**
  - Compose the quote from the fact: "Back door: unlocked, 47 minutes as of 14:24:02Z (checked Nm ago)".
  - When the check is older than a threshold, set the quote in the stale form.
  - Change the empty line to "NOTHING IN THE CONNECTED DESKS NEEDS YOU".
  - Drop the 72vh top-biased hero on phone. It pushes the facts below the fold, and the folio rail with it, at 390px.

## 4. The OpenDesign skill was not really used; the design carries an internal motion contradiction (brief "prompt guidance applied", exclude list)
- **What is wrong.** §0 rejects the skill's motion contract and its page structure. What remains is generic ("single file, no localStorage") plus a "staged text reveal" that §8 then keeps as "staged entry order on first paint". That contradicts the "no motion on by default" exclude and §3's "no slide". Only the design-system tokens are really used. The metaphor and gesture (turning pages between departments) is also a multi-page reading flow, and it comes close to the "concepts on a page" complaint. The Connect lifecycle is a dialog, not part of the paper's front.
- **Fix.**
  - Either name the skill's concrete contribution (for example, the chapter hierarchy as the DEPARTMENT layout) or say plainly that the starting point was the design system alone.
  - Delete the staged entry reveal. Under the exclude list, first paint appears finished.
  - State how the pull-quote slot is chosen when two services need a human, since §3 fixes the order editorially.
