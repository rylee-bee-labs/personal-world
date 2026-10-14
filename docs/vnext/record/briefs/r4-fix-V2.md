# Worlds vNext real round: revise concept V2's DESIGN.md before OpenDesign builds it

## Decisions in force
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept5-variations-brief.md` FIRST (this job), then `docs/vnext/concept5/DESIGN.md` (Concept 5, Rylee's pick), the house design language in `docs/vnext/house/` (DESIGN.md, tokens.css, worlds-mulberry-mint.css, LANGUAGE.md, JOY-CURIOSITY-AND-CARE.md), `docs/vnext/concept-brief-v3.md` and `docs/vnext/concept-brief-v2.md` (still apply), `docs/vnext/fixture/fixture.json` (the only data) and the craft rulebooks in `docs/vnext/od-pack/`. Your variation is V2: the **Welcome-led (onboarding-first)** flow.

Write `docs/vnext/c5var/V2/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section. OpenDesign will build the prototype from it next, so make every decision explicit (both layouts, the live feed, the checker ids). Write only that file. Do not push.

---
## Your DESIGN.md

# V2 — Welcome-led Counter (MOCKUP design)

## 1. Metaphor and main gesture
A small welcome at the service counter: Dot the snail helps the person place the first two service slips, then rests while the counter does its everyday work. Main gesture: **connect a service, then follow one of its facts from source to the people who can see or act on it.** Dot is a quiet inline-SVG accent, never telemetry or a gate.

## 2. Integration model
First run starts with zero connected services and zero providers. “Add a service” opens the six-step Connect flow: Discover → Credential reference → Safe test → Preview → Choose → Publish. Steps are skippable/returnable where safe; skip welcome proceeds directly to the empty Counter. The prototype path discovers and connects fixture `media-shelf` and `front-door` so the home reveal has a quiet Healthy service and a Needs a human service; the chooser never invents facts. Credential fields accept references only, never secret values. Each service remains owned by its named app; Worlds composes its selected facts into the Counter, Memory’s Agent facts view and the service detail. `#add-service`, `#disconnect`, `#simulate-change`, and `#live-toggle` are visible on the first screen, including the welcome/empty state. In the true zero-service state `#disconnect` is named and visible but disabled with “Connect a service before disconnecting”; it becomes enabled for the selected service after connection. Disconnect confirms, removes that service’s tile/facts/frame from Worlds, leaves its owner app untouched, and reports what disappeared.

## 3. Flow and navigation
**Desktop (1440px):** persistent top bar carries MOCKUP, feed controls, `#add-service`, and the selected service’s `#disconnect`; a left rail keeps Home, Connect, Memory and Settings reachable throughout. First-run Home is a broad, calm welcome card with Dot, a short explanation, progress stated honestly as “First service” (no fabricated completion), and a primary “Connect a service” action. Discover leads to credential reference, test, fact preview, publish choice, then a small “Connected” receipt. Dot offers “Add one more” or “See your counter”; after the second publish the home reveal settles into the normal Counter with service slips and selected-service detail. “Skip welcome” enters that same Counter at any point. The flow can be re-entered through Connect; after first run Home opens directly on the Counter.

**Mobile (390px):** the welcome has its own single-column reading order: MOCKUP/title, feed controls, visible `#disconnect` (disabled with its reason until a service exists), Dot and welcome copy, then the 44px-or-larger `#add-service` action. The empty-state line explains that no services are connected yet. Connect becomes a full-screen stepper with named step/progress, one decision per view, and a sticky Back/Next bar replacing the bottom tab bar; the four destinations use a bottom tab bar elsewhere. After publish, the receipt offers “Add one more” and “See your counter”; Home is a vertically scrolling stack of full-width slips followed by the selected service’s receipt and embedded-app frame. At both widths, no horizontal scrolling; text wraps and the control map remains available at 200% zoom.

## 4. Embedded-app frame
Opening a service keeps the Worlds shell, navigation, status and back path visible around a labelled frame for the owning app; the fact receipt stays first and the app frame is secondary. Always include the fixture’s owner fallback link (for example, “Open Home app” → `app://home`). If embedding is unavailable or disabled, preserve facts, state and fallback with a plain explanation and retry/open-owner choices. A real sandbox policy and supported embedding mechanism are UNKNOWN; do not grant the embedded app Worlds credentials. Consequential decisions hand off to the owning system (Project Home for its approvals); Worlds does not act on its behalf.

## 5. Shared facts and live feed
One fact store keyed by fixture fact id drives the Counter, selected service, and Memory/Agent view. Each row identifies value, explicit state, source, checked/observed time and freshness, plus separate “Agent can” and “You can” permissions and their reasons. A `none` permission stays visible as a named disabled item with its reason. `#simulate-change` applies the fixture’s `crossViewExample` (`frontdoor.back-door` becomes locked) atomically across those views; it is a mock handoff, not a real lock action. A mock clock anchored to fixture `generatedAt` recomputes age from `checkedAt`; periodic feed ticks update every freshness display and may visibly mark a fixture observation stale without changing its value or claiming a real reading. `#live-toggle` pauses/resumes that data feed and reports its state in words. Data ticks continue when reduced motion is requested; only decorative transitions stop. Batch any meaningful live announcements politely and never announce routine ticks.

## 6. States
- **Empty / first run:** zero services, zero providers; Dot beside an empty counter, honest invitation to connect, skip welcome. Local Weather remains Not configured with no facts or placeholder value.
- **Quiet:** “Nothing needs you right now”; Healthy and current are explicit words, no celebratory noise.
- **Needs you:** Front Door says “Needs a human” and shows the fixture’s decision reason; the owner retains the action.
- **Stale:** preserve last known value and checked time, label Stale, offer a check/owner route without implying recovery.
- **Unknown:** say there is no current reading and why; never render null as blank or healthy.
- **Disconnected:** confirmation names the service and consequence; afterward report removed Worlds facts/frame and that the owning app remains.
- **Working / failed:** Connect test has static progress, a specific failure, preserved reference, retry and recovery guidance. Status always has a text label; color and Dot expression are supplemental only.

## 7. Borrowed mechanics, distinct flow
Borrow Homarr’s shared integration for interface and agent facts, Dashy’s ability to open an owning app in place, Home Assistant cards’ shared state subscriptions, and MCP ext-apps’ contained app boundary. Deliberately different: this is a warm, optional first-run invitation that earns its welcome through the first two service slips, then settles into a factual Counter. Worlds remains the semantic host; provider UI and permissions do not become Worlds authority.

## 8. OpenDesign and house language
Starting point: OpenDesign `frontend-design` with the assigned `user:rylee-bee-studio` / Rylee-Bee Design System, Mulberry & Mint. Use canonical `--surface-*`, `--text-*`, `--border-*`, `--action-*`, `--focus-ring`, `--status-*`, `--accent-*`, `--space-*`, `--radius-*`, and `--type-*` roles and values from `house/tokens.css`; dark is default, other token modes remain supported. Use Atkinson Hyperlegible Next for body and Young Serif for display, with system fallbacks and no external fonts. Mint is a restrained invitation/primary action; Mulberry surfaces and clear borders carry most of the screen; state words map to Play-Nice language, never color alone. Apply state coverage for empty/loading/failed/unknown, color and typography discipline, visible keyboard focus and targets, restrained motion/reduced-motion, and anti-slop rules: no gradients, emoji icons, filler or invented metrics. Warmth comes from Dot’s small parcel-carrying welcome and precise, kind copy, not forced celebration. Layout is deliberately responsive, with mobile’s stepper and bottom navigation rather than a compressed desktop rail.

## 9. Signature, hero thesis, reference lock
**One signature:** Dot carrying the first service slip into an otherwise quiet counter; after onboarding Dot settles into a small resting accent and the service/fact surface becomes the hero. Thesis: “A first connection should teach the person how Worlds keeps facts and ownership clear.” Reference lock: Homarr 2.0’s reactive widget/integration pairing and Apple Home’s legible accessory state language (mechanics only; no borrowed palette). The distinctive change from Concept 5 is the guided empty → first service → second service → home reveal; the counter slips, Source → Worlds → Agent / You receipt, Dot, six-step Connect and four destinations remain. Compared with C, V2 keeps the persistent shell, owned services, honest shared facts and owner app; it moves the welcome and first-run connection ahead of the composed Counter, then returns Home to the settled Counter for daily use.

## 10. Risks and UNKNOWNs
UNKNOWN: production Connect API/credential-reference semantics, embedded-app sandbox and browser support, disconnect-history policy, live-feed transport/cadence, and whether first-run should connect exactly two services or offer two as a suggested path. The mock flow uses only synthetic fixture data, keeps onboarding skippable, makes no provider calls, and treats cadence/embedding enforcement as prototype boundaries. Verify contrast, focus, reflow, screen-reader order and checker behavior in the eventual prototype at desktop and phone sizes; this design description is not runtime or accessibility verification.


---
## CRITIQUE.md (another engine)

# Critique — V2 "Welcome-led Counter" (MOCKUP design)

Reviewed against `docs/vnext/concept5-variations-brief.md`, `docs/vnext/concept-brief-v3.md`,
`docs/vnext/concept-brief-v2.md`, `docs/vnext/concept5/DESIGN.md`, `docs/vnext/house/` and
`docs/vnext/fixture/fixture.json`. Five gaps, most important first. Nothing here says a decision in
force is wrong; the decisions quoted in the brief are treated as binding.

What V2 already gets right, so the gaps below are read as "not yet", not "wrong": the honest first
run with no fabricated progress; `#disconnect` present-but-disabled *with its reason* in the zero
service state instead of hidden; credential fields that accept references only; fixture ids, names,
state words and the `app://` fallback used verbatim; the owner keeping every consequential decision;
and the counter staying the semantic host while provider UI and permissions do not become Worlds
authority. Those are the right instincts and they should survive the fixes.

---

## Gap 1 — Settings is a destination with nothing in it, and the app never grows past two services

**What is wrong.** `concept-brief-v3.md:13` requires "a settings screen" in the real-application
list. V2 puts Settings in the desktop rail and the mobile tab bar (§3) and then never describes it:
no section, no content, no states. That is the failure Rylee named directly — "they seem to not quite
get the fact it needs to be integrated, they seem like concepts on a page"
(`concept-brief-v2.md:9`) — a fourth tab that leads nowhere.

Three concrete consequences follow:

- **Credential references have a lifecycle in Connect and nowhere else.** §2 stores a reference
  after publish. Nothing can list it, replace it, or remove it, and nothing says what "remove"
  does to the service. `concept5/DESIGN.md:85` modelled all of that, including that removing a
  reference leaves the service "Not configured" with a Reconnect that re-enters Connect at step 2
  with an empty field.
- **`#live-toggle` has no settings twin.** Cadence and the motion preference are named nowhere;
  `concept5/DESIGN.md:87` had `#settings-live` in Settings agreeing with the top-bar toggle at all
  times.
- **The fixture's provider surface is dropped.** `local-weather` (`fixture.json:402-426`) is
  `not_configured` with an `emptyReason` and an `app://settings/providers` fallback. V2 mentions it
  only as a line inside the Empty state bullet (§6), with no screen and no fallback link — even
  though requirement 3 (`concept-brief-v2.md:17`) is about keeping a working route to the owning
  app.

Separately, the integration contract is under-built. Requirement 1 (`concept-brief-v2.md:15`) says
the prototype starts with two connected services and adds a third through Connect; requirement 2
(`:16`) says at least three fixture services, used verbatim. V2 §2 starts at **zero** and the only
prototype path connects exactly two (`media-shelf`, `front-door`). So the settled app ends its life
with two services, requirement 2 is unmet, and Discover names no service at all — which leaves
Preview with no facts to show, since `sync.*` (`fixture.json:370-398`) is the only third service
with a preview worth staging.

**Why it matters.** "A real application, something she would use every day" is the point of this
round. Settings is where a daily-use app earns it: what is connected, what Worlds holds on the
person's behalf, how fresh the feed is, what is not decided yet. Without it, V2 is a beautiful
first ten minutes.

**Concrete fix.** Add a Settings section to DESIGN.md, ordered as: **Connected services** (row per
service, state word, freshness, its own disconnect) → **Credential references** (list, Replace,
Remove, "Worlds stores a credential reference, never the secret."; `ref://{service-id}` handles for
the two welcome-led services declared as UI placeholders, not fixture data; Remove → "Not
configured" + Reconnect into Connect step 2) → **Providers** (Local Weather, `summary` and
`emptyReason` verbatim, fallback "Open provider settings" → `app://settings/providers`, no facts and
no placeholder value) → **Live feed** (`#settings-live`, cadence 5s/10s/30s, "Reduce motion: Follow
system / Always", and the line that reduced motion removes animation, not data) → **About** (MOCKUP,
"synthetic data only", the §10 UNKNOWNs, and a disabled named "Keep history after disconnect" with
its reason). Give the desktop a section sub-list and mobile one scrolling column. Then make the
third service explicit and reachable from the settled counter: `#add-service` stays live after
onboarding and Discover offers **Notes Sync** with its three `sync.*` facts in Preview, so
requirements 1 and 2 are demonstrable on day two as well as on day one. State plainly in §2 that
first run is a variation-imposed change to requirement 1's starting state, and that requirements 1
and 2 are met from the settled counter.

---

## Gap 2 — The one signature is spent on the first five minutes; the daily-use artifact is unnamed

**What is wrong.** The DESIGN.md contract asks for ONE signature element and "the hero is a thesis"
(`concept-brief-v2.md:29-30`). Concept 5's signature is the fact journey receipt and its thesis is
"Every fact says where it came from and who can act on it" (`concept5/DESIGN.md:63`). V2 §9 spends
the ONE signature on "Dot carrying the first service slip into an otherwise quiet counter" — an event
that happens once and, by V2's own words, never again ("after onboarding Dot settles into a small
resting accent").

The receipt survives, but only as inventory. §9 lists "Source → Worlds → Agent / You receipt" among
things that "remain"; §4 demotes the app frame to secondary; and no section says what occupies the
detail pane's primary slot on desktop after the welcome is over, what the three stations are, in
what order they settle, or which view holds the hero. §1 also quietly demotes the main gesture from
Concept 5's "pick a service slip, then open a fact to read its journey" to "connect a service, then
follow one of its facts" — an excellent first-run gesture, and no gesture at all for the other
364 days.

**Why it matters.** The variation brief asks for "a different way a person moves through it" and
says the flow must be obvious within ten seconds (`concept5-variations-brief.md:21`). V2's flow
change is real but confined to first run: same counter, same slips, same receipt, same four
destinations, reached a different way once. From the second day on, V2 is indistinguishable from
Concept 5 and from the counter-first variation. A variation that only differs on day one does not
earn its slot.

**Concrete fix.** Two signatures, labelled by when they apply, and make the daily one V2's own:

- **Onboarding signature (unchanged):** Dot's parcel.
- **Daily signature:** the fact journey receipt, with one twist that only welcome-led can have — the
  receipt's **Worlds** station carries the connection's own provenance for any service connected
  during the welcome: "Connected during first run · reference held, never the secret". That line is
  the welcome's promise made concrete, and it becomes an ordinary row for everything connected
  afterwards, so it earns its place permanently rather than expiring.

Then state the hero per screen and per width — receipt above the app frame on desktop and on mobile,
slip → receipt → frame on desktop, receipt first inside the pushed mobile service screen — and
restore the settle sequence as part of the signature: on a change, the three stations re-settle in
order Source → Worlds → Agent/You, staggered 60ms. Finally, rewrite the second half of §9 to say
what changes from Concept 5 **in daily use**, not only during first run.

---

## Gap 3 — Reactivity is described in four words where the brief asks for a model

**What is wrong.** `concept-brief-v3.md:16` asks for a simulated feed with: a cadence; freshness
changing on a schedule; a service occasionally going stale; every view updating without a reload;
instant optimistic feedback that settles visibly; visible state transitions; and a visible
pause/resume. V2 §5 offers "periodic feed ticks", "may visibly mark a fixture observation stale",
and "`#live-toggle` pauses/resumes that data feed". There is no number anywhere.

Specifically missing:

- **Cadence.** No interval is given, so there is no "checked 12s ago" to observe and nothing for
  Settings to offer 10s/30s against.
- **The re-check model, and the honesty guard that depends on it.** Concept 5 `§14` re-checks live
  services on a schedule, updating `checkedAt`/`observedAt` without changing values, and explicitly
  refuses to re-check anything that is `needs_attention`, `unavailable`, `unknown`, `not_configured`
  or stale. V2 has no such rule, so a tick can age the fixture's `needs_attention` Front Door into
  "Stale" and overwrite fixture truth — the one thing the honesty floor forbids
  (`concept-brief-v2.md:22`, `fixture.json:26`).
- **A determinate staleness event.** "May visibly mark a fixture observation stale" is
  nondeterministic: a reviewer cannot tell a working staleness rule from a missing one.
- **Pause semantics.** No label text, no `aria-pressed`, and no statement of what pausing freezes —
  the clock, the ticks, the re-checks, the staleness event — or what stays usable
  (`concept-brief-v3.md:22` wants the checker to find a real pause).
- **Propagation is incomplete.** §5 lists Counter, selected service and Memory/Agent view. It omits
  the derived attention count, which is part of the fixture's own `crossViewExample`
  (`fixture.json:441`, `attentionCount: 0`). V2 §6 has a "Needs you" state, but nothing on Home
  counts anything, so after `#simulate-change` the person's own attention surface does not visibly
  change.
- **Optimistic feedback is absent entirely** — the word does not appear in V2. Publish and disconnect
  are the two actions a person waits on; both need an instant visible result that then settles.

**Why it matters.** Without numbers, "reactive" is a claim rather than a specification, and the
prototype cannot be built or reviewed against it. The nondeterministic staleness is worse: it can
make a broken feed look like a working one.

**Concrete fix.** Add a live-feed section with the model Concept 5 already proved, plus the
optimistic paragraph:

- Mock clock anchored at fixture `generatedAt` `2026-10-13T14:24:26Z`; first paint "checked 24s ago"
  (Front Door) and "28s ago" (Media Shelf), always recomputed from `checkedAt`, never from
  `ageSeconds`.
- Tick every 5s by default (Settings: 10s / 30s); every tick recomputes every age and re-renders
  slip, header, receipt rows, Memory, Settings and the attention count in one pass.
- Re-check Front Door every 20s and Media Shelf every 30s, setting `checkedAt`/`observedAt` of their
  live facts to the mock now; values do not change. Never re-check a service that is
  `needs_attention`, `unavailable`, `unknown`, `not_configured` or already stale — their ages keep
  growing and their words do not move.
- Staleness event scripted and labelled "simulated": once, Media Shelf, only while Healthy with all
  facts live; state word becomes "Stale", summary "Nothing has changed since {time} that Worlds
  knows of.", "Check now" restores Healthy.
- `#live-toggle` with `aria-pressed`, label "Live · checked 24s ago" when on and "Paused · frozen at
  24s" when off; pausing freezes the clock, ticks, re-checks and the staleness event while actions
  stay usable and `#simulate-change` still works; resume restarts where it stopped.
- `#simulate-change` renders its result in the same frame — `frontdoor.back-door` → "locked", Front
  Door → Healthy, attention count 1 → 0 — with the receipt's stations settling in order, and a line
  "Mock handoff recorded; Worlds locked nothing." staying visible.
- Optimistic: publish and disconnect apply their visible result immediately (new slip in place;
  removed slip gone, its facts and frame with it) and settle visibly, with the removal named in
  words afterwards.

---

## Gap 4 — The 390px layout is the desktop column, stacked

**What is wrong.** `concept-brief-v3.md:15` requires two first-class layouts and says mobile "is not a
squeezed desktop". V2's mobile does have its own chrome — bottom tab bar, full-screen stepper,
sticky Back/Next bar, one bottom strip at a time — but its Home is the desktop's parts in one scroll:
"a vertically scrolling stack of full-width slips followed by the selected service's receipt and
embedded-app frame" (§3).

**Why it matters.** On a phone that is one long page: slips, then a receipt, then a whole embedded
app frame. By the time the reader reaches the receipt they have lost which service they are reading
about, and the app frame — which §4 correctly calls secondary — gets equal weight at the end of the
scroll. Concept 5's mobile map had a *pushed* service screen with in-page tabs for exactly this
reason (`concept5/DESIGN.md:74-80`); V2 removes the push and keeps the tab bar, so the
one-decision-per-view discipline V2 applies so well to Connect never reaches daily reading. Related:
`#live-toggle` and `#simulate-change` appear in §3's mobile reading order only as part of the
welcome/empty screen; nothing says where they live on the settled counter, and the checker wants
`#live-toggle` working everywhere (`concept-brief-v3.md:22`).

**Concrete fix.** Give mobile a pushed service screen and write the whole map down:

1. Page header — MOCKUP and title.
2. One-line status ("1 needs you · checked 24s ago") with `#live-toggle` and `#simulate-change` as
   two 44px-high buttons, wrapping if needed. **This row exists on the settled counter, not only on
   the welcome.**
3. Selected service card — state word, the fixture's reason, "Open Front Door", `#disconnect`.
4. Remaining slips, full width, 44px+ tall.
5. Floating `#add-service`, `bottom: calc(56px + env(safe-area-inset-bottom) + 16px)`,
   `right: 16px`, with the list padded so it never covers a slip; hidden inside Connect.
6. Tapping a slip pushes a full screen: Back in the top bar, tab bar hidden, in-page tabs
   **Facts / App / Agent view** (frame and its fallback link in App, receipt in Facts), `#disconnect`
   under the heading.
7. State the 768px behaviour too, even though it is a bonus.

---

## Gap 5 — Nothing on a working screen moves or warms

**What is wrong.** The decision in force is "the more real application with whimsy and warmth laced
in". `concept-brief-v3.md:14` spells out what that means: "kind micro-copy, a tiny illustration or
character accent drawn in inline SVG, **a gentle motion on a state change**, a satisfying
'connected' moment." V2's entire warmth budget is one parcel carry during onboarding plus welcome
copy, and §8 puts both on the welcome side of the line ("Warmth comes from Dot's small
parcel-carrying welcome"). §9 has Dot "settle into a small resting accent" without saying where it
rests or whether it appears at all afterwards. §6's "no celebratory noise" on Quiet is right and
should stay.

The harder gap is motion: **V2 names no transition anywhere.** No curve, no duration, no state change
that animates. The one line that gestures at it — "only decorative transitions stop" under reduced
motion (§5) — describes a motion design that was never written, and reduced motion without a
designed substitute is a coin flip at build time. `concept-brief-v3.md:16` requires "Transitions
between states are visible (and calm under reduced motion)"; that cannot be built or reviewed from
V2 as it stands. Concept 5 kept Dot on working screens specifically beside the "Updated" line
(`concept5/DESIGN.md:101`) and pinned verbatim copy as the warmth on working screens (`:102`);
`house/JOY-CURIOSITY-AND-CARE.md:160-178` names care visible in little things, and "a satisfying
receipt after something finishes" is one of its examples — which is precisely what V2's "Connected"
receipt should be.

**Why it matters.** Onboarding warmth is a one-time delight; a person on day nine gets a factual
counter with nothing in it. The failure mode is not a theme park — it is the opposite one: the
variation reads as "warm for the demo, blank for the product", which is what the brief is pushing
against.

**Concrete fix.** Add a warmth-and-motion section that pins three things and stays quiet everywhere
else:

1. **Dot's daily placement.** Resting beside the counter's freshness line (visible, never a gate,
   never telemetry), asleep at first run, appearing once beside the "Updated" word when something
   changes, and never on a plain tick.
2. **One motion spec.** A single curve `cubic-bezier(.2,0,0,1)`; state-word confirmation 150ms
   (120ms mobile) with the receipt's stations staggered 60ms Source → Worlds → Agent/You; dialogs and
   sheets 200ms; Dot's parcel 400ms desktop / 300ms mobile. Motion confirms something that has
   already happened; nothing animates on a tick except the number.
3. **The reduced-motion substitute, written down.** No transform, no scale, no travel; changes swap
   with opacity only, the stations show all ticked at once, the connect stamp appears without
   travel, and "Updated just now" is held for 3s. Data keeps updating.

Plus two verbatim lines of kind copy on **working** screens — the stale offer ("Nothing has changed
since {time} that Worlds knows of." with "Check now") and one for a service that drops off — because
precise, kind wording on a working surface is the warmth that survives past minute five. No confetti,
no emoji, no celebration of a Healthy state.

---

## Corrections before the build (not design gaps)

1. **Focus ring — two truths, pick one.** `house/tokens.css:51` gives `--focus-ring: #c4f5e2` in
   dark and `#291d30` in light; `house/worlds-mulberry-mint.css:17` keeps the accessibility
   contract's teal `#72b1b1` in every dark theme, and `concept5-variations-brief.md:16` names the
   teal ring as what Worlds uses today. §8 says use `--focus-ring` from `house/tokens.css`. State
   one source and one value; leaving both will produce a ring that changes colour between Worlds
   surfaces.
2. **"Mint is a restrained invitation/primary action" does not hold in light.** In light mode
   `--action-primary` is `#1f5c47` (`house/tokens.css:92`), not mint. Say mint is the dark-mode
   action colour and read `--action-primary` as written in light, high-contrast and
   low-stimulation.
3. **Language.** Fixture permissions are `agent`/`human` (`fixture.json:74`, `:132-135`), and
   `house/LANGUAGE.md:24-37` is explicit that a human label is a presentation mapping onto a machine
   word, never a second vocabulary. V2 introduces "You can" with no mapping line and uses both "Needs
   you" and the fixture's "Needs a human" in different sections. Either keep Concept 5's
   "Agent can / Human can", or add the mapping sentence: "You can" renders `human`; "Needs you"
   renders `needs_attention`; a service's own `stateLabel` is shown verbatim.
4. **"Project Home" is not a fixture owner.** §4 hands approvals to "Project Home", but the fixture's
   seven owners are Home, Media, Memory, Studio and Settings (the `owner` blocks at `fixture.json:51`,
   `:106`, `:169`, `:224`, `:285`, `:347`, `:409`). Name the owning app as the fixture does and use
   its `appRoute` and `fallback` verbatim, or the handoff names a system the person cannot open.
5. **Heading scale.** `house/tokens.css:39` caps the ladder at `--type-h1: 1.5rem`, and
   `house/DESIGN.md:111` says a ladder past h1 is a product decision composed from those roles. §8
   commits Young Serif for display but never says how the "broad, calm welcome card" heading is
   composed from the six `--type-*` roles. Write that down so the welcome card's size is a decision
   rather than a literal.
