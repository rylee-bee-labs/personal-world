# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c736

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): algorithmic-art dithered — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c736/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c736/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

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
---
## CRITIQUE.md (by another engine)

# Critique of c736 — The Plate (DESIGN.md, not code)

Critic: a different engine from the author. Five gaps, most important first. I checked the §8 contrast figures and they are correct.

## 1. The cross-view demo cannot run from the stated start state (req 4, req 5)
- **What is wrong.** §2 connects `garden-camera` and `media-shelf` at start, and adds `studio-printer` through the lifecycle. §2 also says the governed action is `frontdoor.back-door` → lock. `front-door` is unconnected, sitting in the ADD drawer. At start, no connected service needs a human, so the user sees no attention state and no handoff. §5's hand-off line (`DECIDE IN HOME →`) never appears at start. The sheet opens with a healthy service and an unknown one, so the "needs you" state has to be dug out of the drawer.
- **What is also wrong.** The hand-off is "disabled-by-design". Req 5 says the decision links or hands off to the owner. A disabled control is a dead end, not a link. Nothing says who performs the lock, either: if Worlds does, it breaks req 5.
- **Fix.**
  - Start with `front-door` and `media-shelf` connected, as the other two concepts do. Use `garden-camera` or `studio-printer` as the added service, and disconnect something connected.
  - Make the hand-off a real enabled link to `app://home`.
  - Put the lock inside the embedded Home frame, which sends `postMessage`. Worlds only reflects it through the store.

## 2. The plate is decoration, not integration (brief "the one thing round 1 missed"; req 4)
- **What is wrong.** The seed is `FNV-1a(factId|state)`. The pattern is a pure function of a state word already printed in the cell, so it carries no information the label does not. The `SEED` chip and `RE-PROOF` button demonstrate hash determinism. That is a feature about the art generator and not about services plugging in. Rylee's complaint was "concepts on a page", and a unique canvas texture per fact is the same problem. The hero thesis ("an integration you cannot see is not integrated") is well put but is delivered by the plate, which shows no integration. §6 also admits the dither is a "redundant echo".
- **Fix.**
  - Make the density or seed depend on something real, for example the age bucket from `checkedAt` recomputed at render. Then the plate visibly changes as time passes and when a fact changes.
  - Drop `RE-PROOF`. Show the plate and its word label in the shared-facts row. The proof sheet should show what the owning app reports, not hash plumbing.
  - Make the visible "one fact, many places" element the same fact changing in the cell, the proof sheet and the ledger together.

## 3. The banned indigo ships on screen (brief exclude list: "Tailwind-indigo accent")
- **What is wrong.** §0.2 confines `#8B5CF6` to "dither ink" and calls that a texture. It is still shipped as the visible colour of the plate field, in every cell. The brief bans it with no exception for texture. The reviewer sees violet dots on every plate. The tokens are not at risk here: the DS requires no particular ink.
- **Fix.** Ink the dither with Text `#111827` (or Primary at reduced density). That is inside the DS and invents no hex. Report the conflict to the DS owner as already planned.

## 4. Priority and attention are deliberately flattened, which hides the need that must be found (req 4; floors)
- **What is wrong.** §1 makes every cell identical, with no priority order, so urgency "is carried by a word and a dot density". The Warning dot is `#D97706` at 3.19:1 on white, in a corner of a cell. A "needs a human" cell is therefore one word among a grid of the same word-sized text. The sheet has no attention summary. With 4 columns at desktop and a single column at 390px, a needs-you cell can sit several screens down.
- **Fix.**
  - Add a one-line summary above the sheet, derived from the store ("1 needs a human, 1 unknown"). Make it a link to the first such cell.
  - Keep cell sizes equal if wanted, but sort by state and then name, and say so. A fixed sort is not a "priority order" in the sense the design fears.

## 5. The DESIGN.md contradicts itself on details a builder will trip over (§3, §8, §10)
- **What is wrong.**
  - §8 says an 8×8 Bayer matrix. §10 says "a 4×4 Bayer matrix in a ~28px cell".
  - §8 says cells are "8px-radius square-cornered modules", which cannot both be true.
  - §3 describes the proof sheet as a side panel on desktop but also says cells are buttons with `aria-pressed`-style selection. That is a toggle semantic on something that opens a dialog-like panel.
  - §0.1 says the skill's contribution is reproducibility, and §8 says "the whole of what I take". The skill is therefore barely used.
- **Fix.** Pick one matrix size and one corner radius. Use `aria-expanded` and a real dialog or disclosure pattern for the proof sheet, with focus return. Say plainly that only the design system was used as a starting point, or name a second contribution from `algorithmic-art`.
