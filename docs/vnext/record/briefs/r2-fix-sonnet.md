# Worlds vNext round 2, step 3a: revise your three DESIGN.md files against their critiques

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. For each concept below, write `docs/vnext/round2/<id>/DESIGN.md`: your design revised to fix every gap in its critique, plus a short "Critique fixes" section (how each gap was fixed, or why not). Also name, in the design, the ids the prototype will give its key controls: `#add-service`, `#disconnect`, `#simulate-change`. The prototype will be built from your DESIGN.md by OpenDesign next, so make every decision explicit. Write only the three DESIGN.md files. Do not push.

---
## c151 — your DESIGN.md

# c151 — The Daily Edition (MOCKUP, design only)

Starting point: OpenDesign skill `field-notes-editorial-template` + design system `kami`. Data: `docs/vnext/fixture/fixture.json` only. New concept, not a round-1 evolution.

## 1. Metaphor and main gesture
Worlds is a **printed daily edition** about your own services. Each service is a numbered **article**; the agent-facts view is the **footnotes and colophon**; a service's own app is a **plate** (a figure set into the page).
**Main gesture: turn to a section.** Choose a numbered section from the index and the page changes to that article, like a page view switcher (Front page / Articles / Footnotes), with no scrolling feed.

## 2. Integration model
- A service plugs in as an **article**: headline = its `name`, standfirst = its `summary`, body = its facts as a short ruled table, byline = owner app and freshness.
- Where it lives: the Front page lists articles by section number (1, 2, 3...) in connection order; needs-attention articles also get a "Lead story" slot at the top.
- Facts flow: services write to one ledger; the Front page, article, and footnotes are renders of it. Every fact has a stable footnote number (e.g. note 4 = `frontdoor.back-door`) used in all three places.
- Start: Media Shelf (healthy) and Front Door (needs_attention). **Commission an article** (Connect) adds a third. Steps, as a numbered form on one page: 1 Discover, 2 Credential reference (reference name only, a secret-looking value is refused with a plain note), 3 Safe test (read-only, result printed as a one-line proof), 4 Preview (draft layout of the facts, with proposed see/act), 5 Choose what to publish (tick facts), then "Go to press": the article appears in the index and Front page.

## 3. Navigation
- **Phone:** a masthead, then an **index strip** of numbered tabs that wraps to two rows (no sideways scroll), 44px each: Front, 1, 2, 3, Notes, Add. Selecting changes the page and the hash (`#/n/<id>`); browser Back works.
- **Desktop:** left **contents rail** in the margin (section numbers in ink-blue), article in a single reading column (about 62ch), marginalia column on the right where footnotes sit beside the text they cite.
- Keyboard: index is a tablist (arrows, Home/End); in the article, `[` and `]` go to previous/next section (hint printed in the colophon).

## 4. Embedded-app frame
The service app is a **Plate**: a ruled box inside the article with caption "Plate 2. Front Door, as drawn by its owning app." A header line states the sandbox ("sandboxed; reads nothing from this page"). Below the caption: **Open in the owning app** (`https://example.invalid/home/...`), always shown, and the only thing shown if the plate is unavailable or disabled, with the reason. Worlds keeps: masthead, index, the article's headline/standfirst/status line, and the footnotes beside it. The plate's own UI is a mock and emits events to the ledger.

## 5. Agent/human shared facts
The **Notes** page is a footnote list. Each note: number, label, value, `source`, "checked 28 s ago" (recomputed from `checkedAt`) and two ruled lines, **Agent: may read / may act / may not (reason)** and **Human: may read / may write / may act / may not (reason)**. `none` prints as a struck line with its fixture reason, not removed. Notes cited on an article page also appear in its margin as small refs, so one fact is visible in three places.

## 6. States
- **First run:** the Front page prints "No articles yet" with a one-paragraph colophon ("Worlds works with zero services and zero providers") and one button, Commission an article. Notes page: "No notes."
- **Quiet:** "Nothing needs you" as the lead; articles listed without a lead story.
- **Needs-you:** Lead story slot with the article, a "Hand off" button, count word in the masthead.
- **Stale:** tag reads "Stale" plus the age in words; facts carry a dagger-free marker word "stale" in the table; the table rule turns dashed.
- **Unknown:** values print "unknown" with an em-dash cell; never 0.
- **Disconnected:** the article is replaced by an **erratum**: "Correction: Nightly Backup has been disconnected. Removed: article, 3 notes, one stale warning. Unchanged: its data in Settings." Section numbers close up; a Re-commission link.

## 7. Borrowed vs different
- Homarr: one integration serves tile and assistant. Borrow: article and notes read one record. Different: no tile grid; reading page.
- Dashy: in-place app opening. Borrow: Plate in the page. Different: caption and fallback are printed, permanent.
- HA cards: subscribed state. Borrow: the ledger. Different: the footnote numbers make dependencies legible.
- MCP ext-apps: sandboxed UI. Borrow: Plate as sandbox with message events. Different: the owning app always keeps its decisions; Worlds prints a hand-off.

## 8. OpenDesign starting point and rules applied
- Skill: paper background with soft vignette (kami: none, no gradient, so the vignette is dropped), serif display, page view switcher, count-up on metric numbers (dropped under reduced motion, and only for live values, never for unknown), honest `—` for unknown.
- Design system kami: parchment `#f5f4ed`, ivory `#faf9f5` cards, ink-blue `#1B365D` as the only chromatic color (≤5% of area), warm grays `#141413 / #3d3d3a / #504e49 / #6b6a64`, Charter/Georgia serif at weight 500, **no italic, no bold, no second accent**, 1px rings and whisper shadows, solid-hex tag tints (`#E4ECF5`), tabular numerals. kami has no status colors, so state is carried by **words, rule style (solid/dashed/dotted) and position**, which also satisfies the not-colour-alone floor. Pastel insight cards from the skill are not used because kami forbids a second hue.
- Guide rules: single job (read the day), hero is a thesis, one signature, no AI-indigo, no hero gradient, no invented numbers, motion off by default.

## 9. Signature, hero thesis, reference lock
- **Signature:** footnote numbers that are alive: hover/focus or tap a number and its siblings in the other two places underline together; change a fact and its number gets a small "revised" word.
- **Hero thesis:** "Every claim on this page has a number, and every number tells you who said it and when."
- **Reference lock:** the Economist's Espresso-style numbered briefing and Tufte-style marginal notes (mechanics: side notes, numbering). Not their palettes.

## 10. Risks and UNKNOWNs
- UNKNOWN: whether ink-blue-only reads enough urgency for needs-you; mitigated by position (Lead story) and words, to be checked in screenshots.
- Serif at weight 500 only: loss of bold emphasis; use size and small caps via letter-spacing.
- Margin notes collapse on phone to inline "Notes for this article" list; behaviour UNKNOWN until built.
- Charter is not guaranteed offline; falls back to Georgia. CJK variants are out of scope.
- Real ext-app sandbox behaviour UNKNOWN. Hand-off hrefs use `example.invalid`.


## c151 — CRITIQUE.md

# Critique — c151 "The Daily Edition"

By a different engine (round 2, never one-shot). Checked against
`docs/vnext/concept-brief-v2.md` (requirements 1–6, Floors) and
`docs/vnext/fixture/fixture.json`. Five gaps, most important first.

## Gap 1 — The plug-in names no service and no discoverable source (Req 1, Req 2)

**What is missing.** §2 says Connect "adds a third," and the lifecycle is
ticked off as numbered steps, but the design never says *which* service the
prototype wires and never says where Discover's list comes from. Req 1 fixes
the lifecycle shape; Req 2 requires using the fixture's ids, names and states
"verbatim" and adding nothing it does not contain.

**Why it matters.** This is the exact thing round 1 was criticised for: the
integration is *described*, not specified, so the builder cannot wire Discover
without inventing service names or guessing the third service — the one place
"add nothing it does not contain" is easiest to breach. A reviewer also cannot
tell whether the connected third is Nightly Backup (stale) or something else,
so the states the built prototype will actually show are unknowable.

**Concrete fix.** In §2, state the source: Discover lists the fixture's
remaining services — Nightly Backup, Garden Camera, Studio Printer, Notes Sync,
Local Weather — with their fixture `name` and `state` verbatim, and name the one
service the built prototype adds, with its `id`.

## Gap 2 — "One fact, many places" is an architecture claim, not a demo, and it drops the service's own surface (Req 4; fixture `crossViewExample`)

**What is wrong.** §2 says the Front page, the article and the footnotes are
"renders of" one ledger; §5 says one fact is visible in three places. All three
views are Worlds-side. Req 4 requires that one change update, *at once*, the
home, **that service's own surface**, and the agent-facts view. Here the plate
only "emits events to the ledger" (§4): it is an input, never a view that
redraws. The fixture even ships a worked dependency (`frontdoor.back-door` lock
→ `front-door.state` healthy → `attentionCount` 0), and the design never uses it.

**Why it matters.** Req 4 is the crux of "genuinely reactive UI" (owner
feedback on #322) and the thing round 1 missed. "Rendered in three places" is
architecture; nothing in the design forces the masthead count, the lead-story
slot and the Notes page to move together, and no action in the whole concept is
the one that changes a fact.

**Concrete fix.** Make the built demo the fixture dependency: the plate (or the
returned hand-off) posts the back-door lock; specify exactly which surfaces
redraw from the same store — the Front page lead story and count word, the
article's status line, the Notes page row and its "revised" word — and that the
plate is one of them.

## Gap 3 — The governed hand-off has no destination, and Worlds is never stated to abstain (Req 5)

**What is missing.** §6 gives a "Hand off" button in Needs-you but never says
where it goes. Req 5: a decision is never made in Worlds; it links or hands off
to the owning system — Project Home for approvals, the service itself otherwise.
§5 lists "Human: may act" for facts like `frontdoor.back-door`, but nothing says
that act *leaves* Worlds.

**Why it matters.** Front Door's back door is a real human `act` fact, and the
fixture's rule is that the owning app keeps authority. With no destination
named, the design reads as Worlds performing the lock — the opposite of
"external systems keep their own authority."

**Concrete fix.** Name the hand-off per case (approvals → Project Home; act
facts and `retry` → the owning app's route, e.g. `app://home`) and state that
Worlds only records the returned result.

## Gap 4 — The state model omits two states the design's own Connect flow produces (Req 2, Req 6; fixture `statesVocabulary`)

**What is missing.** §6 covers first-run, quiet, needs-you, stale, unknown and
disconnected, but has nothing for an `unavailable` service (Studio Printer,
Notes Sync — both carry a `retry` action) or a `not_configured` service with
zero facts (Local Weather). If Discover offers the fixture services named above,
those states will occur and must render.

**Why it matters.** The fixture's state vocabulary includes both, and
zero-provider usability is an owner invariant. An article with *no facts* has no
defined page at all, so the design cannot honestly compose Local Weather as it
stands — and `unavailable` is exactly where the retry hand-off of Req 5 lives.

**Concrete fix.** Add `unavailable` (with the fixture `retry` offered as a
hand-off, never run in Worlds) and `not_configured` (an empty article that
prints the fixture `emptyReason`, never a placeholder value) to §6 — or state
plainly that the built prototype connects only Media Shelf, Front Door and
Nightly Backup and Discover shows only those.

## Gap 5 — The signature and the margin notes have no keyboard/AT path, and phone margin behaviour is deferred (Floors: keyboard reach, visible focus)

**What is missing.** The signature (§9) is hover/focus/tap cross-highlighting
of footnote numbers "in the other two places," but §3 guarantees the keyboard
only for the index tablist and `[`/`]`. There is no stated keyboard route from
a Notes-page fact to the article margin, no announcement for the cross-highlight,
and §10 admits the phone margin collapse is "UNKNOWN until built."

**Why it matters.** The floor is "every interaction reachable by keyboard with
visible focus," and a signature that only works on hover is not reachable. Leaving
the phone behaviour as UNKNOWN leaves a required viewport undemonstrable.

**Concrete fix.** Make the footnote number one focusable control repeated in all
three places, with the cross-reference announced on focus (a polite live region),
and replace the phone UNKNOWN with a defined collapse to an inline "Notes for
this article" list, checked at 390px.

---
## c365 — your DESIGN.md

# c365 — The Sentence (MOCKUP, design only)

Starting point: OpenDesign skill `high-end-visual-design` + design system `user:rylee-bee-studio`. Data: `docs/vnext/fixture/fixture.json` only. Not an evolution of a round-1 concept; new.

## 1. Metaphor and main gesture
Worlds is a concierge who tells you the state of your house in **one living sentence**, and the services are the clauses of it. "Back door unlocked for 47 minutes, Nightly Backup is stale, and Media Shelf is fine." Each clause is a button.
**Main gesture: pull a clause forward.** Activating a clause lifts that service's card out of a slightly rotated stack and opens it beside the sentence. The sentence stays and re-writes itself whenever a fact changes.

## 2. Integration model
- A service is a **clause + a card**. The clause is generated from the service's own `state`, `stateLabel` and `summary` (never from Worlds-side data). The card holds its facts, owner app, and fallback.
- Where it lives: the clause sits in the sentence in urgency order (needs_attention, unavailable, stale, unknown, healthy; not_configured and disabled go in a closing clause "and N not set up"). The card sits in the stack.
- Flow: the service pushes `state`, `freshness` and `facts` into one store. Sentence, stack, dock badge and Facts drawer are all pure renders of that store, so one change redraws all four.
- Start: Media Shelf (healthy) and Front Door (needs_attention) connected. Connect adds a 3rd (default offer: Nightly Backup; the other fixture services are also listed under Discover).
- Connect is a bottom sheet in five steps: Discover (list of fixture services not yet connected) → Credential reference (a name like `ref:backup-reader`, field refuses anything that looks like a secret value, says so) → Safe test (read-only probe, shows the result in words) → Preview (the facts and capabilities it offers, each with proposed see/act) → Publish (checkboxes per fact; "Publish N of M") → the clause appears in the sentence with a short settle.

## 3. Navigation (Worlds <-> service)
- **Phone (390px):** sentence on top (large serif), stack of cards below it, a floating pill **dock** (not edge-to-edge) with: Home, Facts, Connect. Tapping a clause opens its card as a full-height sheet; the dock stays, and a "Back to the sentence" pill is the first control in the sheet. Browser Back closes the sheet (hash route `#/svc/<id>`).
- **Desktop:** editorial split. Sentence fills the left half; the right half is the stack, and an opened card widens to hold the embedded frame. Same dock, floating at bottom centre.
- Keyboard: clauses are links in reading order; Enter opens, Esc returns focus to the clause that opened it. Dock is a toolbar with arrow-key movement.

## 4. Embedded-app frame
A framed app drawn as a nested **double-bezel** (outer tray, inner glass) holding the service's own UI (Media Shelf: a small library list; Front Door: its lock panel; Nightly Backup: its run history). Sandbox is stated on a slim strip above it ("sandboxed, no access to Worlds data"). Beneath: **Open in the owning app** -> `https://example.invalid/<app>/...`, always visible, and shown alone, with the reason, if the frame is "unavailable" or "disabled". Worlds keeps around it: the sentence (still live), the dock, the back pill, and a status line for that service (state word + freshness) that is Worlds's own, not the app's. Embedded UI is a drawn mock; it posts events into the store, as an ext-app would.

## 5. Agent/human shared facts
The **Facts** drawer is a table of every fact: label, value, `source` (local/external/none), freshness ("checked 28 s ago", recomputed from `checkedAt`, never `ageSeconds`), then two separate pairs of pills: **Agent: sees / acts**, **Human: sees / acts**. A permission of `none` is drawn as a disabled pill with its fixture reason (e.g. `guest-code-used`, agent: "Identifying a person is human-only"). A fact's row shows a "changed just now" word when it just moved. Filter chips: All / Agent can act / Human can act / Not live.

## 6. States
- **Zero services, zero providers:** the sentence reads "Nothing is connected yet. Worlds works without it." with one pill, Connect a service. No cards, no fake numbers. Facts drawer says "no facts".
- **Quiet:** everything healthy: "Nothing needs you." plus the quiet clauses; no urgency styling.
- **Needs-you:** the clause leads the sentence, the dock badge shows "1 needs you" in words, the card has a hand-off button.
- **Stale / Unknown:** words and dashed vs dotted rules; Unknown facts show "unknown, never read", never 0 or blank. Camera "Unknown" is spoken as unknown, not offline.
- **Disconnected:** the clause is replaced for the session by a plain line: "Nightly Backup disconnected. Gone from Worlds: its 3 facts, its card, its stale warning. Still true: its data and settings live in Settings." Reconnect pill offered.

## 7. Borrowed vs different
- Homarr: widget = view over one integration, reused for UI and assistant. Borrow the one-integration-two-readers idea. Different: no widget grid; the home is prose.
- Dashy: open apps in place. Borrow in-place opening. Different: Worlds stays visible around the app (sentence + dock).
- HA custom cards: shared state subscription. Borrow the single store. Different: every card also states freshness and actor permissions.
- MCP ext-apps: sandboxed tool UI. Borrow the sandbox + event posting. Different: the fallback link is a first-class control, not an error path.

## 8. OpenDesign starting point and rules applied
- Skill: Editorial Split layout archetype, Z-axis cascade (stack, rotations removed below 768px), double-bezel, floating pill island dock (it bans glued navbars), eyebrow pills, custom cubic-bezier motion, no thick icons. Not taken: Inter-class fonts, glass blur heroes, big gradient orbs, `py-40` macro padding on a phone.
- Design system: Rylee-Bee tokens by role (`surface/base|raised|sunken`, `text/primary|soft`, `border/line|line-strong`, `action/primary`, `focus/ring`, `status/*`, `accent/mint|apricot|lilac|honey`), rem only, no clamp, dark default, light and high-contrast modes, status as Play-Nice words. Painted sticker icons are not available offline, so icons are thin inline SVG always beside a word.
- Guide rules: single job (what needs me, in a sentence), hero is a thesis (the sentence), one signature element, no emoji icons, no invented metrics, no indigo, motion off under `prefers-reduced-motion`. State-coverage and accessibility-baseline checked per section 6.

## 9. Signature, hero thesis, reference lock
- **Signature:** the sentence that rewrites itself; changed words get a 600 ms underline settle (none under reduced motion, replaced by "updated" text).
- **Hero thesis:** "A house is one sentence long when it is fine, and a short list when it is not."
- **Reference lock:** Linear's issue-view calm and Apple Weather's one-line summary (mechanics: one line first, detail on pull). Not their palettes.

## 10. Risks and UNKNOWNs
- UNKNOWN: Rylee-Bee hex values and the display typeface; `tokens.css` is not in the pack, so the build reads `design/tokens.json` and uses system serif/sans stacks (no external fonts).
- Sentence generation can read awkwardly with 7 clauses; cap at 3 named clauses plus "and N more", checked in build.
- Screen readers: a rewriting sentence needs a polite live region that announces only the changed clause; to test.
- Embedded UIs are drawn mocks; real sandbox behaviour (iframe `sandbox`, postMessage) is UNKNOWN.
- Hand-off hrefs use `example.invalid`; fixture `app://` routes are shown as route text only.


## c365 — CRITIQUE.md

# Critique — c365 "The Sentence"

By a different engine (round 2, never one-shot). Checked against
`docs/vnext/concept-brief-v2.md` (requirements 1–6, Floors) and
`docs/vnext/fixture/fixture.json`. Five gaps, most important first.

## Gap 1 — The assigned design system has no real value source: it reads a file that holds names only, in a different schema (guide lever 1: bind to the assigned system; do not invent values outside it)

**What is wrong.** §10 says "tokens.css is not in the pack, so the build reads
`design/tokens.json`." That file is the Personal World token **names** contract —
its header says "Never put hex values here," it contains zero hex, and its
groups are `void/canvas/hull/panel/elevated/raised` and accent
`primary/secondary/warm/...`. The assigned system, Rylee-Bee, uses a different
vocabulary (`surface/base|raised|sunken`, `text/primary|soft`, `action/primary`,
`focus/ring`, `status/*`). So §8's "Rylee-Bee tokens by role" have no source in
the pack, and the builder will produce either nothing or private hex literals.

**Why it matters.** Lever 1 is explicit that a concept binds to its assigned
system and invents no values outside it; a source that yields no values means
every colour is a literal that "escaped review." The same drift shows in §8's
icons: the system's rule is the painted B Classic sticker set "always beside a
word," but the design substitutes thin inline SVG. (The `high-end-visual-design`
skill is present in the pack and its layout/motion mechanics are quoted
accurately — the problem is the *system* binding, not the skill.)

**Concrete fix.** Say where Rylee-Bee values actually come from (for example the
theme JSON under `design/themes/`, or declare a provisional palette explicitly
and cite the role names plus a measured contrast check), and either use the
system's icon rule or record the inline-SVG substitution as a named deviation.

## Gap 2 — The governed hand-off has no destination, and Worlds is never stated to abstain (Req 5)

**What is missing.** §6 Needs-you says "the card has a hand-off button"; §4 lists
a Worlds-side status line. Neither names where the hand-off goes. The fixture's
Front Door carries a human `act` fact (`frontdoor.back-door`), and Studio Printer
and Notes Sync carry `retry` actions — all of which belong to the owning system.
Req 5 says a decision is never made in Worlds; it links to Project Home (for
approvals) or the service itself.

**Why it matters.** Without a destination the sentence/card reads as Worlds
performing the lock or the retry, which contradicts the invariant that external
systems keep their own authority.

**Concrete fix.** Name the target per case (approvals → Project Home; act and
retry → the owning app route) and state that Worlds records the returned result
only.

## Gap 3 — Floors and the exclude list are only partly met: 44px targets unstated and motion on by default (Floors; Exclude list)

**What is wrong.** §3 says "Phone (390px)" but never states the floor's **44px
targets** or **no sideways scroll**, and the floating dock (Home/Facts/Connect)
and clauses have no stated target size. §9's signature is a **600 ms** underline
settle, on by default; the brief's exclude list forbids "motion on by default,"
and `animation-discipline.md` lints "more than 500 ms on any non-cross-screen
transition" — a word-change confirmation is not a cross-screen morph.

**Why it matters.** These are fixed constraints, not preferences. c151 and c975
both state 44px; c365 does not. A 600 ms settle makes the reader wait on an
animation that is not carrying the meaning — the new sentence already does.

**Concrete fix.** State 44px minimum for dock items, clauses and pills, and a
no-horizontal-scroll guarantee at 390px; set the settle at ≤200 ms (or off by
default) and reserve motion for the sheet open (≤300 ms). Say explicitly which
of the skill's mandatory animations ("no element appears statically") are dropped
because this is a working tool, not a landing page.

## Gap 4 — The state model omits `unavailable` and `not_configured`, though Discover lists those fixture services (Req 2, Req 6; honesty)

**What is missing.** §6 covers zero services, quiet, needs-you, stale/unknown and
disconnected. It has no `unavailable` (Studio Printer, Notes Sync) and no
`not_configured` (Local Weather). But §2 says "the other fixture services are
also listed under Discover," so the built prototype can produce both.

**Why it matters.** The fixture's vocabulary includes both, and an
`unavailable` service is where the retry hand-off lives; a `not_configured`
service has **zero facts**, and the design has no defined rendering for that
(the Facts drawer's "no facts" is only tied to first-run). The honesty floor
("a blank value is never a state") needs the empty case spoken, not implied.

**Concrete fix.** Add `unavailable` (retry as a hand-off) and `not_configured`
(an empty clause/card that prints the fixture `emptyReason`) to §6, or restrict
Discover to the three connected services.

## Gap 5 — Uniqueness: the "one surface that rewrites itself" spine duplicates round-1 `the-desk`, and the design never says what differs (Anti-repeat list)

**What is wrong.** §1 and §9 make the thesis "one living sentence … the sentence
stays and re-writes itself whenever a fact changes." Round 1's `the-desk` is
"one sheet of writing that is always being rewritten … the line, its margin note
and the tally all answer in one pass," and it sits in the anti-repeat set unless
an evolution becomes a real integrated shell. §7 compares c365 only to
Homarr/Dashy/HA/MCP; the anti-repeat list is not addressed at all.

**Why it matters.** A copy of another concept is not allowed, and a new author's
sentence surface reading as a restyle of `the-desk` is exactly the "concepts on
a page" failure the owner called out. The differentiator has to be on the record
before the build.

**Concrete fix.** Add one paragraph naming `the-desk` and the concrete
differences — clauses-of-one-sentence as controls, a per-service stack, the
embedded frame, and the governed hand-off — and say what spine change (if any)
keeps it from being a restyle.

---
## c975 — your DESIGN.md

# c975 — The Triage Chart (MOCKUP, design only)

Starting point: OpenDesign skill `hand-drawn-diagrams` + design system `doodle`. Data: `docs/vnext/fixture/fixture.json` only. New concept; not an evolution of switchboard, strata or orrery.

## 1. Metaphor and main gesture
Worlds is a **hand-drawn flowchart that answers questions about your services**. The home is not a map of connections; it is a decision chart: "Does anything need a human?" "Is anything out of date?" "Is anything broken?" Each service is a sketched box that sits at the end of the branch its current state answers.
**Main gesture: follow the arrow.** Step through the chart question by question (tap a question, or arrow keys along the path); a box is opened by following its arrow into it. When a fact changes, the box **re-routes**: its arrow redraws to the branch its new state belongs to.

## 2. Integration model
- A service is a **box** with its name, a state word, and one summary line (all from the fixture). Its branch is derived only from its `state`: needs_attention -> Q1 yes; stale/unknown -> Q2 yes; unavailable -> Q3 yes; healthy -> "All fine"; not_configured/disabled -> a dashed "Not set up" box at the foot.
- Facts flow: one store; boxes, the open sheet, the Facts board and the chart's counts ("1 needs you") read from it, so one change redraws all.
- Start: Media Shelf and Front Door connected. **Add a box** (Connect) is a drawing step: a dashed empty box labelled "+ new service" on the chart opens a 5-step side sheet, each step ticked off like a checklist drawn on the board: Discover -> Credential reference (reference name, secret-looking text refused) -> Safe test (read-only; result in words) -> Preview (facts and capabilities, proposed see/act) -> Publish (tick facts). On Publish the dashed box "inks in" and its arrow routes to the right branch.

## 3. Navigation
- **Phone (390px):** the chart is a vertical flow in one column (questions as speech-bubble headers, boxes beneath each answer, arrows pointing down). A bottom tab row of three 44px tabs: Chart, Facts board, Add. Opening a box opens a full-width "pinned page" over the chart with a "Back to chart" first control; hash route `#/box/<id>`.
- **Desktop:** chart on the left (two answer columns under each question), the pinned page opens at the right so the chart stays visible and keeps re-routing live.
- Keyboard: boxes are in path order; Enter opens, Esc closes and returns focus to the box; the path has a visible focus ring plus a "step 2 of 4" word.

## 4. Embedded-app frame
The service app is a **pinned page**: a taped-corner rectangle on the board labelled "EMBEDDED: Front Door (owned by Home)". Sandbox note in the label ("sandboxed, cannot read the chart"). **Open in the owning app** -> `https://example.invalid/...` is a drawn button on the page, always visible; if the app is unavailable or disabled the page shows only the fallback and the reason. Worlds keeps: the chart (live), the tab row, the back control, and a status stamp for the service (state word, freshness). The mock app UI posts events to the store.

## 5. Agent/human shared facts
The **Facts board** is a sheet of sticky labels, one per fact: label, value, source stamp (LOCAL / EXTERNAL / NONE), "checked 28 s ago" (from `checkedAt`), and below, two separate rows: **Agent** and **Human**, each with drawn tick-marks and words for see / act (human also write). `none` is a crossed-out label with its fixture reason, still visible. A fact that just changed wears a "just changed" tag. Tapping a label highlights the box that owns it on the chart.

## 6. States
- **First run:** an empty board: question Q1 with a dashed empty box and the line "Nothing connected. Worlds still works." and an Add button. Zero providers is the same state.
- **Quiet:** Q1 answers "No" with a plain tick; all boxes under "All fine".
- **Needs-you:** Q1 answers "Yes: 1", box under it, with a "Hand off" button on its page. The chart's answer is written in words.
- **Stale:** the box has a dashed outline and the word "Stale" plus age.
- **Unknown:** dotted outline, "?" glyph beside the word "Unknown", values "unknown".
- **Disconnected:** the box is erased to a ghost outline, labelled "Disconnected. Gone: 3 facts, 1 warning. Kept: your data in Settings." Counts update; chart still works.

## 7. Borrowed vs different
- Homarr: widgets reuse one integration for UI and assistant. Borrow: box and Facts label share one record. Different: no widget grid; the layout is an answer to questions.
- Dashy: apps open in place. Borrow: pinned page. Different: chart persists beside it and re-routes.
- HA cards: shared subscription. Borrow: store. Different: routing is derived from state, so the layout itself is the subscription.
- MCP ext-apps: sandboxed UI. Borrow: sandbox label + events. Different: fallback link is always drawn.

## 8. OpenDesign starting point and rules applied
- Skill `hand-drawn-diagrams` (catalogue entry only; upstream not read here, so UNKNOWN beyond: Excalidraw-style rough boxes, arrows, animated SVG). Used: rough-stroke SVG boxes, arrows that draw in on change, no PNG/hosted edit link.
- Design system doodle: primary `#49B6E5` for the interaction signal (focus, primary button, active arrow), secondary `#263D5B` for strokes and text outlines, text `#111827`, surface `#FFFFFF`, success `#16A34A`, warning `#D97706`, danger `#DC2626` used only as accents beside words, spacing 4/8/12/16/24/32, type 14/16/18/24/32/40, Delius Swash Caps for display (UNKNOWN offline; fallback `"Segoe Print","Bradley Hand",cursive` for headings only) and a plain system sans body for legibility, JetBrains Mono for stamps with monospace fallback. 150-250 ms transitions.
- Guide rules: single job (answer "what do I do?"), hero thesis, one signature, no emoji icons (hand-drawn SVG glyphs), no invented metrics, reduced motion = arrows appear fully drawn.

## 9. Signature, hero thesis, reference lock
- **Signature:** the re-routing arrow: when a service's state changes, its arrow is erased and redrawn to the new branch.
- **Hero thesis:** "The layout is the answer: where a box sits tells you what to do about it."
- **Reference lock:** Excalidraw's rough-stroke look and a St. Louis-style paper decision tree (mechanics: question nodes, branch labels). Not palettes.

## 10. Risks and UNKNOWNs
- Looks risk: Excalidraw clones and "playful" can read as unserious next to a door-lock decision; counter with plain labels.
- Rough filters (SVG turbulence) can hurt legibility and perf; wobble only on strokes, never on text.
- Doodle colors on white: warning amber `#D97706` on white may miss text contrast; use it only for strokes beside words, to verify.
- Doodle font unavailable offline: fallback rendering UNKNOWN across devices.
- With 7 services the chart gets long on phone; collapse "All fine" and "Not set up" groups.
- Real ext-app sandbox behaviour UNKNOWN. Hand-off hrefs use `example.invalid`.


## c975 — CRITIQUE.md

# Critique — c975 "The Triage Chart"

By a different engine (round 2, never one-shot). Checked against
`docs/vnext/concept-brief-v2.md` (requirements 1–6, Floors) and
`docs/vnext/fixture/fixture.json`. Five gaps, most important first.

## Gap 1 — The plug-in names no service and no discoverable source (Req 1, Req 2)

**What is missing.** §2 describes the five-step side sheet (Discover →
Credential reference → Safe test → Preview → Publish) but never says what
Discover lists or which third service the prototype adds. §6 first-run only says
"Nothing connected." Req 1 fixes the lifecycle shape; Req 2 requires the
fixture's ids, names and states "verbatim" and adding nothing it does not contain.

**Why it matters.** This is the round-1 failure the brief was written to fix —
the plug-in is drawn as a step list rather than specified, so the builder must
invent the source list and the added service, and a reviewer cannot tell which
fixture states the built chart will actually show.

**Concrete fix.** State the source: Discover lists Nightly Backup, Garden
Camera, Studio Printer, Notes Sync and Local Weather with their fixture
`name`/`state` verbatim, and name the one service the built prototype adds,
with its `id`.

## Gap 2 — Routing is derived from service state only, so the chart can contradict the facts board (Req 4; honesty)

**What is wrong.** §2 routes each box "only from its `state`": `stale`/`unknown`
→ Q2, `unavailable` → Q3, `healthy` → "All fine." But the fixture's services
carry mixed *fact* states — `notes-sync` is `unavailable` while
`sync.local-pending` is `live`; `studio-printer` is `unavailable` while
`printer.queue` and `printer.last-error-code` are `live`. Req 4 makes the fact
the unit: each fact has its own source, freshness and permissions.

**Why it matters.** A box that answers "Is anything broken? Yes" while its Facts
board shows live facts is the kind of disagreement the concept's own
"one change redraws all" promise is meant to prevent, and it can overstate or
hide a fact's real state. Because the chart's counts ("1 needs you") are service
counts, a mixed service is invisible in the answer.

**Concrete fix.** State how fact-level states roll up into the box (for example
worst-fact-wins, named) and render the mixed cases (`notes-sync`,
`studio-printer`) explicitly, so the chart's answer and the Facts board cannot
disagree.

## Gap 3 — The governed hand-off has no destination, and Worlds is never stated to abstain (Req 5)

**What is missing.** §6 Needs-you puts a "Hand off" button on the box's page but
never says where it goes. Front Door's `frontdoor.back-door` is a human `act`
fact, and Studio Printer / Notes Sync carry `retry` actions — all owned by the
external system. Req 5: a decision is never made in Worlds; it links to Project
Home (approvals) or the service itself.

**Why it matters.** With no destination, the box reads as Worlds locking the
door or retrying the job, against the invariant that the owning app keeps
authority.

**Concrete fix.** Name the target per case (approvals → Project Home; act and
retry → the owning app route) and state Worlds records the result only.

## Gap 4 — Uniqueness vs the anti-repeat list is unaddressed: boxes on branches read as a tech tree (Anti-repeat list)

**What is wrong.** §7 compares c975 only to Homarr / Dashy / HA cards / MCP
ext-apps. The core — sketched boxes sitting at the ends of branches, arrows
re-routing — sits next to the anti-repeat list's **tech trees** and round-1's
`foldout-atlas` (an invented map with a traced route). The design never says why
a decision flowchart is not a tech tree.

**Why it matters.** "Do not reproduce" is a brief rule, and a hand-drawn
Excalidraw-look chart with node-and-arrow geometry is the shape most likely to
read as a banned tree or as the round-1 atlas restyled. The counter has to be
argued in DESIGN.md before the build, not discovered in a screenshot.

**Concrete fix.** Add a uniqueness paragraph: the layout is *an answer to
questions* — a position is a verdict, not a level; routing is derived from live
state, with no unlocks or progression; and confirm it is not `foldout-atlas`
with a rougher stroke.

## Gap 5 — The chart has no text alternative (accessibility-baseline: charts and SVG data viz need a long-form alternative)

**What is missing.** The chart is an SVG decision diagram, but the design gives
only a drawn equivalent — boxes, arrows, speech-bubble questions — plus a
"step 2 of 4" word. `accessibility-baseline.md` requires a long-form text
alternative for charts and SVG data viz; a screen-reader user cannot walk
"follow the arrow" from the drawing alone.

**Why it matters.** The Floors demand keyboard reach with visible focus, and the
craft baseline states plainly that a chart without a text alternative is
unreadable to a screen reader. The path order helps navigation but does not
convey the chart's content.

**Concrete fix.** Ship an ordered text outline of the chart — each question and
the boxes under each answer, with the counts and state words — as the accessible
equivalent, and give the drawn chart an accessible name that points to it.
