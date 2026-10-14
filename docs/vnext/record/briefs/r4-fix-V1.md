# Worlds vNext real round: revise concept V1's DESIGN.md before OpenDesign builds it

## Decisions in force
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept5-variations-brief.md` FIRST (this job), then `docs/vnext/concept5/DESIGN.md` (Concept 5, Rylee's pick), the house design language in `docs/vnext/house/` (DESIGN.md, tokens.css, worlds-mulberry-mint.css, LANGUAGE.md, JOY-CURIOSITY-AND-CARE.md), `docs/vnext/concept-brief-v3.md` and `docs/vnext/concept-brief-v2.md` (still apply), `docs/vnext/fixture/fixture.json` (the only data) and the craft rulebooks in `docs/vnext/od-pack/`. Your variation is V1: the **Counter-first (home-first)** flow.

Write `docs/vnext/c5var/V1/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section. OpenDesign will build the prototype from it next, so make every decision explicit (both layouts, the live feed, the checker ids). Write only that file. Do not push.

---
## Your DESIGN.md

# c5var V1: Counter-first (home-first) (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing is a real reading. A visible "MOCKUP" label sits in the top bar (desktop) and page header (mobile) on every screen. This is Concept 5 ("The Counter", Dot the snail) dressed in the house language (Rylee-Bee, Mulberry & Mint), with the flow organised around one rule: **the app opens on the composed counter, everything starts from a slip, and Connect is a side action.** Concept 5 §2-§6, §9, §13, §14 and §15 (integration model, six-step Connect, fact journey, honest fact rows, states, Settings, live feed, Dot) are kept unless named below.

## 1. Flow, step by step
Empty → connect → composed home → embedded app → a change → disconnect. Here the order of *attention* is home first; the empty and connect steps are reached from home, not before it.

**Desktop, 1440:**
1. Open: the counter. Top bar (MOCKUP, `#live-toggle` "Live · checked 24s ago", `#simulate-change`, `#add-service`, `#attention-count` "1 needs you"), left rail (Home, Connect, Memory, Settings), a slip column with Front Door pre-selected ("Needs a human", with `attentionReason`) above Media Shelf, and the detail pane. Within ten seconds: slips on the left, the selected one's story on the right.
2. Pane for the selected slip: header with state word and **`#disconnect` ("Disconnect Front Door") visible on the first screen**, then the fact journey receipt (Source → Worlds → Agent / You), the "Agent sees" strip, then the app frame strip with its fallback ("Open Home app").
3. Open a fact row: its three stations expand in place. `frontdoor.guest-code-used` shows the named, disabled control with the fixture's reason.
4. Change: `#simulate-change` applies `lock-back-door`; slip, header, receipt, agent strip and `#attention-count` all re-settle in one render; "Mock handoff recorded; Worlds locked nothing." stays. If nothing else needs you: "That's the last one. Nothing needs you right now."
5. Connect (side action): `#add-service` opens Connect as a **side panel over the pane** (not a route away), six steps with `#step-back`, `#step-next`, `#run-test`, `#publish`. The counter stays visible and live behind it; on Publish Dot carries the parcel and a Notes Sync slip lands in the column, selected.
6. Disconnect: `#disconnect` → confirm dialog (Disconnect / Cancel) → "{Name}'s {n} facts and its app frame are removed from Worlds; the app itself is untouched." Focus goes to the next slip. Disconnect both and the counter shows Dot asleep with the first-run line and `#add-service`.

**Mobile, 390** (own control map; one bottom strip at a time, per Concept 5 §3):
1. Page header (MOCKUP, title), then the status row: "1 needs you · checked 24s ago", `#live-toggle` and `#simulate-change` as two 44px buttons.
2. Selected-service card (Front Door): state word, reason, "Open Front Door", **`#disconnect` on the first screen**.
3. The other slips, full width. Floating `#add-service` bottom-right above the tab bar (safe-area aware), list padded so it never covers a slip.
4. Tap a slip → pushed full screen (Back in top bar, tab bar hidden), same status row and `#disconnect` under the heading, in-page tabs App / Facts / Agent view.
5. Connect: a full-screen stepper with a sticky Back/Next bar replacing the tab bar. Disconnect: bottom sheet with scrim. Tab bar: Home, Connect, Memory, Settings.

## 2. What changed from Concept 5, and why
- **Skin:** Wise lime/black-on-off-white is replaced by the house language, because Rylee asked to bring Concept 5 into "our design language". Structure and copy are unchanged.
- **Home is the only start:** there is no first-run takeover or guided intro (that is the Welcome-led variation). First run is a state of home, not a mode.
- **Connect becomes a side action** (side panel on desktop, pushed stepper on mobile) so a person can see the counter keep ticking while they connect. In Concept 5 it was a peer destination; the rail entry stays so it is still reachable.
- **Slips are the entry for everything:** open, disconnect, change and Settings deep links all start from a slip; Settings' per-service `#disconnect-{service-id}` rows remain.

## 3. House tokens
Copy `:root` and the dark block from `house/tokens.css`; dark is default, light/high-contrast/low-stimulation blocks follow it. No colours, spacing or radii outside it.
- Surfaces: `--surface-base` page, `--surface-raised` slips and panes, `--surface-sunken` the app frame and inset receipt. Text `--text-primary`, secondary `--text-soft` (soft contrast, never pure white). Borders `--border-line`; `--border-line-strong` for the selected slip.
- Primary buttons (`#add-service`, Next, Publish): `--action-primary` fill with `--action-on-primary` text. Focus: 3px ring, `--focus-ring` token, with Worlds' teal `#72b1b1` from `worlds-mulberry-mint.css` as the front-door ring if the build keeps that contract; recheck ≥3:1 on all three surfaces.
- State words always beside a shape and a label, never colour alone: Needs a human `--status-needs_attention`, Healthy `--status-healthy`, Stale `--status-stale`, Unknown `--status-unknown`, Unavailable `--status-unavailable`, Not configured `--status-not_configured`. Human labels follow `LANGUAGE.md` (Needs you = `needs_attention`); no new state is invented.
- Selected slip: raised surface + 2px `--border-line-strong` + the word "Selected" + a check shape.
- Spacing `--space-*`, radii `--radius-md` (rows) / `--radius-lg` (cards, frame) / `--radius-full` (pills), type `--type-*` (nothing under `--type-small`). Lengths in rem. Targets 44px, aliased from Play-Nice as `--target-min` (not a house token). Fonts: Atkinson Hyperlegible Next (body) and Young Serif (display, service headings) first, system fallbacks, no external fonts.
- Warmth per `JOY-CURIOSITY-AND-CARE.md`: Dot and kind copy appear as invitations after the machine truth, never creating urgency. Motion: one curve `cubic-bezier(.2,0,0,1)`, 150ms state confirms, 200ms dialogs; reduced motion (system or Settings) means no transform, instant swaps, data still updates.

## 4. States
First run (Dot asleep, "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you."), quiet, needs-you, stale (Media Shelf simulated, "Check now"), unknown ("Worlds has no current reading for this and won't guess."), error (failed safe test via "Simulate a failed test (mock)": `lastError` "provider timed out", try N of 3), disconnected, loading ("Dot is fetching the facts…" plus fallback link), empty preview, long copy wraps. Wording is Concept 5 §6 verbatim. Start data: exactly `media-shelf` and `front-door`; Notes Sync is added only by Connect.

## 5. Live feed
Concept 5 §14 unchanged: mock clock anchored at `generatedAt` (first paint "24s ago", ages recomputed from `checkedAt`), 5s tick (Settings 10s/30s), re-check Front Door 20s and Media Shelf 30s, one simulated Media Shelf stale event at 90s, never overwriting fixture states. `#live-toggle` (`aria-pressed`, on by default) pauses clock, ticks and events; `#simulate-change` still works paused; `#settings-live` always agrees with it. One store keyed by fact id, one render pass updates slip, pane, receipt, agent strip, Memory, Settings and `#attention-count`; feed changes carry the word "simulated". Home-first adds one rule: the open Connect panel never pauses or blocks the tick.

## 6. Integration requirements met
(1) Connect adds Notes Sync live through discover → reference (`ref://` only, secrets refused) → test → preview → choose → publish; (2) three-plus fixture services composed; (3) owner app frame with fallback links verbatim, shell navigation intact; (4) one change updates Home, service surface, Memory's Agent facts and Settings at once, with Agent can / Human can as separate lines; (5) approvals hand off to Project Home, other actions to the owner app; (6) disconnect says plainly what is gone and Worlds keeps working.
Checker ids: `#add-service`, `#disconnect` (first screen, selected service), `#simulate-change`, `#live-toggle`, plus MOCKUP label. Floors: 1440 and 390 designed on purpose (768 bonus), no sideways scroll, 44px targets, keyboard focus visible, focus moves to the new heading on push and back to the invoking slip.

UNKNOWN: embed isolation, Connect API, disconnect history, font fidelity; the focus ring choice (house vs Worlds teal) is flagged for Rylee.


---
## CRITIQUE.md (another engine)

# c5var V1 "Counter-first (home-first)" — critique

Reviewer pass on `docs/vnext/c5var/V1/DESIGN.md` against `concept5-variations-brief.md`,
`concept-brief-v2.md`, `concept-brief-v3.md`, `concept5/DESIGN.md`, `docs/vnext/house/` and
`docs/vnext/fixture/fixture.json`. At most five gaps, most important first. Not graded generously.

## Gap 1 — The variation's defining flow is asserted, not designed, and the rule in §2 contradicts V1's own control map

**What is missing or wrong.** The brief says each variation "is the whole app, but organised around a
different way a person moves through it" and must "make the flow obvious within 10 seconds"
(`concept5-variations-brief.md`, "The variations"). V1's §1 desktop and mobile layouts are Concept 5
§12's layouts — 232px rail, 360px slip column, detail pane; the same mobile control map of status row,
selected-service card, other slips, floating `#add-service`, pushed service screen. The only visible
differences from Concept 5 are the removed intro and Connect sitting in a panel, so a reviewer opening
V1 beside Concept 5 sees the same first screen and nearly the same journey; "home-first" is a claim in
prose, not a testable screen.

Worse, the defining rule in §2 — "Slips are the entry for everything: open, disconnect, change and
Settings deep links all start from a slip" — is falsified by V1's own §1: `#simulate-change` and
`#add-service` are top-bar buttons, Settings is a rail/tab-bar destination, and on the empty counter
(§4 first run) there is no slip at all, so the first Connect cannot start from one.

**Why it matters.** The assigned flow is the one thing this file exists to pin. As written, a build can
reskin Concept 5's Connect and satisfy every sentence while the brief's "different overall flow" and
"obvious within 10 seconds" requirements go unmet; the checker's `#add-service` intent ("starts the
Connect flow") is also ambiguous once the rule says everything starts from a slip.

**Concrete fix.** State the split explicitly: shell actions live in the top bar/rail
(`#add-service`, `#simulate-change`, `#live-toggle`, Memory, Settings); the slip is the entry for a
*service* (select → its pane → facts, actions, `#disconnect`). Give the empty/first-run counter its
named Connect entry (the Dot-sleep line's `#add-service`, not a slip) and say so. Add one testable
sentence for what makes V1 itself different on the first screen — e.g. "the counter is composed before
you may connect, and the only route into a service is through its slip" — and make the layout show it,
rather than inheriting Concept 5 §12's order intact.

## Gap 2 — "Connect as a side action" has no interaction contract

**What is missing or wrong.** The brief assigns counter-first "Connect is a side action"
(`concept5-variations-brief.md`). V1 §1 step 5 says the panel opens over the pane and "the counter
stays visible and live behind it", but §1–§4 never state the panel's width or how the six-step stepper
lays out inside it, whether it is modal, how focus is contained, what Escape does, where focus returns,
whether Home stays current, or what happens to the top bar's `#simulate-change`/`#live-toggle` while it
is open. The Preview and Choose steps (a checkbox per fact, "what is shared", Concept 5 §2) were
designed for Concept 5's full Connect area and are not re-sized here.

**Why it matters.** This is the only structural change the variation introduces, and it is exactly the
surface where the floors bite: "every interaction reachable by keyboard with visible focus… focus moves
to the new heading on push and returns to the invoking slip" (`concept5/DESIGN.md` §3, kept; floors in
`concept-brief-v3.md` §3). A non-modal panel over a live counter is the easiest way to ship a focus trap
that is not one, or a panel that silently pauses the feed the brief insists keeps running.

**Concrete fix.** Pin it: panel width and step layout at 1440 (raised surface, `--radius-lg`, the six
steps with `#step-back`/`#step-next`/`#run-test`/`#publish`), whether it is a labelled region or
`role="dialog" aria-modal="false"`, focus moves in on open and returns to `#add-service` on close,
Escape closes and returns focus, Home stays `aria-current`, and the top-bar feed controls remain
operable. For mobile, say the pushed full-screen stepper is the *same* six steps and the same focus
rules (V1 §1 mobile step 5 already implies it — make it explicit), so the two layouts cannot drift.

## Gap 3 — House language is mis-applied in two named places, and V1 contradicts itself

**What is missing or wrong.**
(a) §3 says "Focus: 3px ring, `--focus-ring` token, with Worlds' teal `#72b1b1` from
`worlds-mulberry-mint.css` as the front-door ring if the build keeps that contract", and the UNKNOWN
list flags "the focus ring choice (house vs Worlds teal) … for Rylee". But the brief lists the teal
`#72b1b1` ring as part of *how Worlds applies the house language* — a product-wide focus ring, not a
front-door-only accent (`concept5-variations-brief.md`, "Our design language"; `worlds-mulberry-mint.css`
defines `--focus: #72b1b1` "kept in every dark theme"). V1 both narrows it to one service and refuses
to commit; its `3px` ring and `2px` border are also `px` literals where the house rule is "every length
is `rem`" (`house/DESIGN.md`, "Structural roles").
(b) §3 says "Human labels follow `LANGUAGE.md` (Needs you = `needs_attention`)", but the list directly
above it and all of §1/§4 ship "Needs a human" with `--status-needs_attention` — neither the word nor
the token LANGUAGE.md maps (`LANGUAGE.md`: "Needs you | `needs_attention`"; `--status-needs-you`).

**Why it matters.** The brief's instruction is to "bring Concept 5 into our design language… use these
token names and values… do not invent colours, spacing or radii outside them". A ring decision deferred
to Rylee and a state word the file says it follows but does not leave the build with no single source
for either.

**Concrete fix.** Commit the ring: alias the Worlds teal into one product focus handle used on every
interactive element, express the width in `rem`, state the ≥3:1 check against `--surface-base`,
`--surface-raised` and `--surface-sunken`, and drop the flag from UNKNOWN (the brief already answers
it). For the state word, say which wins and why: if the fixture's `stateLabel` "Needs a human" is kept
verbatim per concept-brief-v2 requirement 2, call it the presentation of `needs_attention` and stop
claiming LANGUAGE.md's "Needs you"; if LANGUAGE.md wins, change the label. Do not leave both in the same
paragraph.

## Gap 4 — The daily-use shell is inherited by reference, not designed

**What is missing or wrong.** Brief v3 §1 asks for "a real application, something she would use every
day… real navigation, a settings screen, a first-run / empty state, an error state". V1 names Settings
twice (§2 "Settings' per-service `#disconnect-{service-id}` rows remain"; §4 states) and folds
everything else into its opening clause — "§13 … kept unless named below". Nowhere does V1 say how
Settings, Memory, or the drop-off error state actually look at 1440 and 390 inside the home-first flow,
nor how the Connect panel reaches Settings › Credential references when the counter is behind it
(Concept 5 §2's only Settings deep link). §4's error state is only the failed safe test; brief v3 §1
also wants "a service that fails its test **or drops off**".

**Why it matters.** The reactive counter is the strongest part of this file; the shell a person lives
in daily is the weakest. "Quality comes from decision coverage, not word count"
(`concept-brief-v2.md`, prompt guidance) means "Concept 5 §13, kept" is not a decision for *this* flow,
where navigation has changed and the panel adds a second way in.

**Concrete fix.** Add a short section pinning, for 1440 and 390: Settings' section list and where it
sits in the home-first order; Memory as the aggregate Agent-facts surface reachable from the rail/tab
bar; the drop-off error state with its copy and recovery; and the deep link from the Connect panel's
done screen into Settings › Credential references, with how focus returns to the counter afterwards.

## Gap 5 — Required brief sections are missing (difference from C; OpenDesign; borrowed vs different)

**What is missing or wrong.** `concept-brief-v3.md` requires "every concept's DESIGN.md must say in one
paragraph how it differs from C, and what it keeps"; `concept-brief-v2.md` §7–§8 still apply (borrowed
from Homarr / Dashy / HA cards / MCP ext-apps and what is deliberately different; the OpenDesign
starting point and which guide rules were applied). V1 keeps Concept 5 §2–§6, §9, §13–§15 — dropping
§7, §8 and §11 — and §2 compares only against Concept 5. None of the three required comparisons
appears.

**Why it matters.** "Everything in `concept-brief-v2.md` and `concept-brief-v3.md` still applies"
(`concept5-variations-brief.md`). A variation that cannot say what it keeps from C and how it differs
from it reads as a reskin in review, and the OpenDesign / borrow note is how the round's "make unique
things of course" requirement is evidenced.

**Concrete fix.** Add one paragraph: what V1 keeps from C (the persistent Home / Connect / Memory /
Settings shell, labelled owned services, the fact journey, the verbatim fixture reasons and fallbacks)
and the one thing that differs (C docks an app beside a static card and nothing further happens per
fact; V1 opens a slip whose fact rows are interactive three-station journeys that visibly re-settle).
Then a two-line OpenDesign note (skill + design system, rules applied) and the borrow/different
sentence.

