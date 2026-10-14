# Worlds vNext round 2, step 2: critique three DESIGN.md files (another author's)

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- Never one-shot: every design gets a critique by a different engine before it is built. You are that critic.

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. Below are three DESIGN.md files by another author. For EACH, write `docs/vnext/round2/<concept-id>/CRITIQUE.md` with AT MOST 5 gaps, most important first. Each gap: what is missing or wrong against the brief (cite the requirement number or floor), why it matters, and the concrete fix. Focus on integration (plug-in live, composition, embedded frame, one fact many places, governed handoff, disconnect), honesty (stale/unknown, no invented data), uniqueness vs the anti-repeat list, and whether the OD skill/design system was really used. Do not rewrite the design; do not grade generously; if a design has fewer than 5 real gaps, list fewer. Write only the three CRITIQUE.md files. Do not push.


---
## DESIGN.md for c151

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

---
## DESIGN.md for c365

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

---
## DESIGN.md for c975

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
