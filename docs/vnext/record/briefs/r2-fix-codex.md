# Worlds vNext round 2, step 3a: revise your three DESIGN.md files against their critiques

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. For each concept below, write `docs/vnext/round2/<id>/DESIGN.md`: your design revised to fix every gap in its critique, plus a short "Critique fixes" section (how each gap was fixed, or why not). Also name, in the design, the ids the prototype will give its key controls: `#add-service`, `#disconnect`, `#simulate-change`. The prototype will be built from your DESIGN.md by OpenDesign next, so make every decision explicit. Write only the three DESIGN.md files. Do not push.

---
## c258 — your DESIGN.md

# c258 — Service stage

## 1. Metaphor in one line, and the main gesture.
Worlds is a small architectural stage: each independent service is a placed volume, and opening one changes the camera from the shared composition into that service's own room. The main gesture is **choose a service in the stage index, then enter its framed room**.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Connect is a six-step track: discover → credential reference only → safe test → preview offered facts/capabilities → choose what to publish → service joins the stage. The initial composition uses fixture services `garden-camera` (Garden Camera, unknown), `studio-printer` (Studio Printer, unavailable), and `notes-sync` (Notes Sync, unavailable). Each service owns its own data and actions; Worlds places a modest service marker and publishes selected facts into the shared facts view. One changed observation updates its own room surface, the Home composition, and the agent ledger by shared fact identity, with source and freshness retained. The stage does not synthesize a state from geometry.

## 3. Navigation between Worlds and a service (phone and desktop).
Desktop Home is a wide plan-like composition with three unequal service bays, plus a stable shell header for Home, Connect, Memory, Settings. Selecting a bay opens the service room as a right-side framed pane while the plan remains behind it. Phone converts bays into a vertical, ordered service index; selecting a service pushes a full-width room with an always-visible “Worlds / Home” breadcrumb and Back button. Focus moves to the room heading and returns to the selected service control on exit. The same order and actions work by keyboard; no spatial drag or perspective gesture is required.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The room is an isolated embedded surface owned by the service. The frame edge carries the service name and textual state; Worlds header, navigation, and Back stay outside and available. Worlds mediates no service commands inside the frame. Sandbox attributes, origin policy, and app-to-shell messaging are UNKNOWN. If embedding is unavailable, replace the frame with a plain explanation and the owner's explicit `https://example.invalid/...` fallback link; exact path is UNKNOWN. A keyboard-accessible “Open in owning app” link is always available.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
An “Observations” drawer lists the same fact records used by service rooms and Home. Each record includes source, observed/checked time, freshness words, agent visibility, human visibility, and actor. `sync.provider-reachable` is unavailable from external source; `sync.local-pending` remains a local live fact, so the two must not collapse into one service status. `printer.queue` remains readable while `printer.status` is unavailable. Agents can read only facts allowed by fixture permissions; a `none` permission is shown as unavailable to that actor with its reason. Human actions hand off to the owning app; approvals hand off to Project Home.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
With zero services, an empty stage offers “Connect a service” and explains the local-only baseline; zero providers cannot create placeholder readings. Quiet is a composed stage with no attention claim beyond fixture evidence. Needs-you appears only when an explicit fixture state/reason supports it; this selected set has no `needs_attention` service, so this composition has none. Stale is shown in words with last-known time; unknown Garden Camera reports the fixture's “no current observation” reason. Unavailable is distinct and preserves local facts like pending notes. Disconnect removes a bay and room, names what is gone, and leaves shell navigation and other services intact; historic observation policy is UNKNOWN. Loading/error/edge layouts must be added before prototype review; no perpetual animation, and reduced motion removes spatial travel.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's service-as-widget composition, Dashy's open-in-place app behavior, HA's explicit unavailable/unknown entity states, and MCP ext-apps' isolated UI plus host-mediated permission boundary. Deliberately, the stage has no cable graph, map, orbit, or freeform spatial editing; composition is a fixed reading order, not an operational topology. Reference mechanics are from the repository note and remain unverified against live external sources.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `swiss-creative-mode-template` + `perspective`. Adapt the skill's scene-based navigation, hotspots/details, and strong editorial rhythm into a product shell rather than a slide deck; omit theme cycling and palette cycling because they add no product value. Use Perspective's spatial depth as a shallow, static hierarchy: layered planes distinguish shell, service bay, and embedded room, with its named scale and spacing. Its white surface needs reconciliation with the accessibility contract's no-pure-white-large-surface and low-glare floor; final values UNKNOWN. Follow anti-slop, typography, AA contrast, state coverage, 44px targets, semantic reading order, and reduced-motion rules; perspective never carries status by itself.

## 9. Signature element, hero thesis, reference lock.
Signature: a service bay that opens into an in-context room without losing the surrounding Worlds frame. Hero thesis: “Independent services can share a home without sharing authority.” Reference lock: Apple Finder's spatially stable sidebar-to-content navigation (mechanic only) and Home Assistant's entity cards for explicit state (mechanic only). The stage composition is the hero artifact; the app room is its proof.

## 10. Risks and UNKNOWNs.
Perspective depth can imply relationships or importance that fixture data does not establish; use only enough layering to show shell containment. The assigned system's white surfaces and saturated green require contrast and comfort validation. Actual service embedding, safe sandbox policy, Connect manifest/API, selection persistence, disconnect history, and screen-reader behavior of nested service apps are UNKNOWN. Only fixture values may be shown. The planned spatial transition is optional, short, and absent under reduced motion. This is a design plan, not a tested prototype.


## c258 — CRITIQUE.md

# CRITIQUE — c258 "Service stage"

Critic engine: MiniMax-M3.1-Flash-Preview (not the author). Scope: `DESIGN.md`
for c258 against `docs/vnext/concept-brief-v2.md`, `docs/vnext/fixture/fixture.json`,
`docs/vnext/design-research.md` and `docs/vnext/od-pack/`. 5 gaps, most important
first. The design is not rewritten here.

## 1. The composed set cannot demonstrate requirement 4 or 5 at all

**Wrong.** §2 composes `garden-camera` (unknown), `studio-printer`
(unavailable) and `notes-sync` (unavailable). That set contains **no fact with an
`act` permission, no fact with a `none` permission, and no `needs_attention`
service**. §5 nevertheless writes the rules for "a `none` permission is shown as
unavailable to that actor with its reason" and "approvals hand off to Project
Home", and §6 says needs-you cannot appear in this composition.

**Why it matters.** Brief requirement 4 asks the agent-facts view to show
*who can see* and *who can act* separately; requirement 5 asks for a governed
handoff. In the fixture those two rules live only on `front-door`:
`frontdoor.back-door` (`human: act`, with `humanReason`) and
`frontdoor.guest-code-used` (`agent: none`, with `agentReason`). The fixture's own
`permissionVocabulary.rule` makes `none` a *named, disabled control with its
reason*, never hidden — a rule this concept states and then never exercises. The
needs-you state in §6 is likewise dead copy. A concept can legally pick any three
services (requirement 2), but picking a set where the two hardest requirements
are unrenderable turns the honest-state machinery in §5/§6 into decoration.

**Fix.** Put `front-door` in the composed set (e.g. keep `garden-camera` for
unknown and `front-door` for needs_attention; drop one of the two `unavailable`
services). Then name the fixture's `crossViewExample` — action
`lock-back-door` on `frontdoor.back-door` — as the prototype's one-fact-many-places
demonstration, with the resulting `front-door.state: healthy` and
`attentionCount: 0` shown simultaneously on the bay, the Home composition and the
agent ledger. Bind the disabled-with-reason rule to `frontdoor.guest-code-used`
specifically, and record that `studio-printer`'s `retry.allowed: human` is the
non-approval handoff case.

## 2. Requirement 1 has no start state and no named third service

**Wrong.** Requirement 1 is "starts with 2 connected services and lets the user
add a 3rd through the Connect lifecycle". §2 says "The initial composition uses
fixture services `garden-camera`, `studio-printer`, `notes-sync`" — three already
present — and the Connect track ends with the unlabelled "service joins the
stage". §5 then discusses `printer.queue` and `printer.status` as if Studio
Printer were already composed. So the document simultaneously presents three
services at start and describes one arriving.

**Why it matters.** The build contract is that the prototype demonstrates
requirements 1–6 with real JS. As written, the Connect lifecycle has no defined
end state: which service appears, with which fact IDs, in which state, on which
surface. This is exactly the round-1 failure the owner named ("concepts on a
page") reappearing as an unpinned number.

**Fix.** Pin it: start with `garden-camera` + `front-door`; the Connect rail adds
`studio-printer` and the last step publishes `printer.status` (unavailable),
`printer.queue` (1 job), `printer.last-error-code` (`E-MEDIA-02`) into the shared
views. Write the six steps as states the prototype renders (with the
credential-*reference* field visibly rejecting a secret value, per
`credential reference (never a secret value)`), not as a one-line list.

## 3. The fallback link is declared UNKNOWN when the fixture supplies it verbatim

**Wrong.** §4 says the owner fallback is "the owner's explicit
`https://example.invalid/...` fallback link; exact path is UNKNOWN". The fixture
gives exact fallbacks for every service: `notes-sync` → label "Open Memory app",
href `app://memory`; `studio-printer` → "Open Studio printers",
`app://studio/printers`; `garden-camera` → "Open Home cameras",
`app://home/cameras`. The fixture's `_meta.syntheticNote` is explicit: "Links use
the `app://` scheme only".

**Why it matters.** Requirement 3 asks for a *visible* fallback link. Treating the
href as UNKNOWN either means the design did not read the fixture, or it defers a
decision that will be resolved by inventing a URL at build time — which is what
the floor "no invented data" forbids. Only the real embed capability is UNKNOWN;
the link is not.

**Fix.** Replace the paragraph with the three fixture `owner.fallback.label` /
`href` pairs verbatim, state that the link is rendered inside the embedded frame
and stays visible at all times, and reserve the word UNKNOWN for sandbox
attributes, origin policy and app-to-shell messaging (which §4 already lists
correctly).

## 4. Unequal bays and layered depth assert a priority the fixture does not have

**Wrong.** §2 specifies "three unequal service bays" and §8 uses perspective depth
to "distinguish shell, service bay, and embedded room". §10 then concedes that
"perspective depth can imply relationships or importance that fixture data does
not establish".

**Why it matters.** The concession is the gap. In the composed set the three
services are unknown / unavailable / unavailable — nothing in the fixture ranks
them — so unequal bays are decoration that reads as a claim. Round 1 already
spent its spatial-plane budget on `field-atlas map` and `foldout-atlas` (places
that unfold, with an app window docked in the map); the "stage" as currently
specified is a master–detail layout with three weights on it, and the one idea
that could distinguish it from that round-1 family (containment depth) is the one
idea §10 disowns. The brief allows an evolution of the author's own round-1
concept only if it "becomes a real integrated shell" — the spatial layer is not
what makes it integrated.

**Fix.** Make the three bays equal and let the fixture's state words carry all
hierarchy, keeping depth strictly to two values (shell plane, room plane) with no
size or offset difference between services; or, if unequal bays stay, derive them
from the fixture — the one justified prominence is `needs_attention`, which
requires fix #1 first. Delete the equal-width-vs-depth ambiguity by pinning it in
§2 as a measurable rule.

## 5. The assigned skill is a deck template, and the deck skeleton was kept

**Wrong.** §8 names `swiss-creative-mode-template`. Its od-pack front matter
(`od-pack/skill-swiss-creative-mode-template.md`) is `surface: video`,
`type: hyperframes`, with "interactive slide navigation", "theme switching" and
"palette choreography" as its core mechanics. §8 keeps "scene-based navigation"
and drops only theme/palette cycling — the navigation model is the deck's central
mechanic, and §1 builds the metaphor on it: "opening one changes the camera from
the shared composition into that service's own room".

**Why it matters.** Requirement 3 demands that Worlds stay the frame: navigation,
back and status keep working *around* the embedded app. A scene/camera model is
the opposite skeleton — it swaps the whole composition for the next scene and has
no persistent shell to survive the embedded frame. §10 also leaves the assigned
`ds-perspective` tokens ("final values UNKNOWN", "its white surface needs
reconciliation"), which defers lever 1; the design system's values are known
(`#00BD7D` primary, `#D97706` warning, `#DC2626` danger) and the decision to
reconcile them against the accessibility floor is a design decision this document
is supposed to have made.

**Fix.** State precisely what is borrowed from the skill — the grid, the type
scale, hotspot/disclosure discipline — and state that navigation is shell-route
based with the header, back control and status rail permanently mounted while the
service room opens inside them. Then pin the reconciled `ds-perspective` tokens
(surface value, accent, and the contrast check against
`od-pack/accessibility-baseline.md`) instead of marking them UNKNOWN; if the green
primary cannot meet the floor, say which role it loses.
---
## c407 — your DESIGN.md

# c407 — Service cabinet

## 1. Metaphor in one line, and the main gesture.
Worlds is a well-made service cabinet: each service has a labeled physical-feeling bay, while a shared index card carries the facts that matter across the shell. The main gesture is **pull a labeled service drawer forward to inspect its app and facts**; keyboard users activate the equivalent “Open” button.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Connect works as a guided intake card: discover → add a credential reference, never its value → safe test → preview offered facts/capabilities → choose what to publish → add the service bay. The initial cabinet holds `media-shelf` (Media Shelf, healthy), `notes-sync` (Notes Sync, unavailable), and `local-weather` (Local Weather, not_configured). A service owns its records, UI, and commands. Worlds shows published facts as shared index cards keyed by fixture fact ID; one observation change redraws the service bay, Home summary, and agent-facts view from that record. Unconfigured Local Weather stays factless and explains why.

## 3. Navigation between Worlds and a service (phone and desktop).
Desktop uses a left cabinet index grouped by the shell's four areas and a broad service work surface; selecting a service opens its drawer-like pane alongside its shared fact card. Home, Connect, Memory, and Settings remain persistent. On a 390px phone, the index becomes a simple vertical list, and opening a service navigates to a single-column detail screen with explicit Back and shell destination controls. No drag-to-open or hover affordance is required. Focus enters the detail heading, then returns to its invoking control. Every target is at least 44px; layout must reflow at 200% zoom.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The drawer's inset screen is the service's own app in a contained frame, visually distinct from Worlds controls. A tactile press/release may confirm opening, but never controls state alone; default motion remains static. The frame is sandboxed with no ambient access to Worlds credentials; the exact policy and service protocol are UNKNOWN. Shell header, back path, service state, and owner identity remain outside. If the embedded app cannot load, its area becomes a plain fallback with a visible owner link at `https://example.invalid/...`; the final path is UNKNOWN. Service actions remain in the service; approvals route to Project Home.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
The cabinet's removable index card is the shared fact surface, not a second copy of service truth. It names source, checked/observed time, freshness, “Agent can see,” “Human can see,” and who may act. `media.items` is readable by both; no act permission is inferred. `sync.provider-reachable` can be unavailable while `sync.local-pending` remains live. `weather` contains no facts because no provider is configured. The card shows a disabled named control and reason when an actor has `none`; the agent never sees a fact whose permission is none. Any action request is a handoff to the owner, not a decision in Worlds.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
Zero services shows one empty cabinet bay and the Connect invitation; zero providers still leaves local Worlds and Memory available without invented readings. Quiet uses the healthy Media Shelf and does not claim more. The chosen set contains no needs_attention service, so do not invent a needs-you alert. Stale and unknown remain textual states wherever fixture observations warrant them; unavailable Notes Sync preserves its live local pending count and stale last-sync separately. Not configured Local Weather says there is no provider and shows no value. Disconnect removes the bay and framed app, clearly names the removed service, and preserves the remaining cabinet and shell. Whether old fact history remains is UNKNOWN. Loading/error/long-copy states need implementation coverage. No ambient motion; reduced motion strips drawer travel.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's independent service surfaces, Dashy's file-like service configuration and in-place app opening, HA cards' explicit unavailable/unknown values, and MCP ext-apps' contained app and host permission check. Deliberately, the cabinet is not a room metaphor or a visual network: its bays are a simple list of independent owners, and its tactile cue adds recognition without implying mechanical control. External mechanics in the research note have not been live-verified.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `design-taste-frontend` + `skeumorphism`. The taste skill itself says it is not for dashboards or multi-step product UI, so borrow its brief-first inference and anti-default discipline only; the product shell requirements take precedence over marketing-page rules. Read this as a calm personal appliance, with low variance, low motion, and moderate information density. Use the assigned skeuomorphic family for one restrained tactile inset frame and raised/pressed affordance, not stacks of ornamental cards. Its vivid orange is reserved to one action cue and must meet contrast; its pure white surface is softened to satisfy the accessibility floor, with exact tokens UNKNOWN. Apply typography discipline, status labels, no invented metrics, full state coverage, keyboard equivalence, 44px targets, and reduced-motion support.

## 9. Signature element, hero thesis, reference lock.
Signature: a physical-feeling inset service screen nested within a persistent Worlds cabinet, paired with a detachable shared-fact card. Hero thesis: “The shell gives every service a place and gives every fact a source.” Reference lock: Teenage Engineering OP-1's labeled physical control surfaces (mechanic and material cue only) and Apple Home's accessory detail-to-home handoff (mechanic only). The cabinet is the characteristic artifact; tactile depth must not obscure service ownership.

## 10. Risks and UNKNOWNs.
Skeuomorphic texture can add noise, reduce contrast, and suggest affordances the prototype cannot honor; keep depth to one frame and one pressed state. Taste-skill marketing rules conflict with the requested multi-step product shell and are not applied where out of scope. Exact accessible color tokens, iframe capabilities/sandbox, service protocol, Connect details, and disconnect-history behavior are UNKNOWN. The selected fixture subset has no needs_attention state, so that state must be described but not fabricated in this concept's populated view. This is a design plan, not a tested prototype.


## c407 — CRITIQUE.md

# CRITIQUE — c407 "Service cabinet"

Critic engine: MiniMax-M3.1-Flash-Preview (not the author). Scope: `DESIGN.md`
for c407 against `docs/vnext/concept-brief-v2.md`, `docs/vnext/fixture/fixture.json`,
`docs/vnext/design-research.md` and `docs/vnext/od-pack/`. 5 gaps, most important
first. The design is not rewritten here.

## 1. The composed set makes requirements 4 and 5 untestable

**Wrong.** §2 composes `media-shelf` (healthy), `notes-sync` (unavailable) and
`local-weather` (not_configured). Every fact in that set is `agent: read` /
`human: read`; `local-weather` has no facts at all. §5 then specifies "the card
shows a disabled named control and reason when an actor has `none`", "the agent
never sees a fact whose permission is none" and "approvals route to Project
Home", and §6 confirms "the chosen set contains no needs_attention service".

**Why it matters.** Requirement 4's payload is *who can see* and *who can act*,
separately; requirement 5 is a governed handoff to Project Home. Both are
test-only on `front-door` in the fixture — `frontdoor.back-door` is the only row
with `human: act` plus a `humanReason`, and `frontdoor.guest-code-used` is the
only row with `agent: none` plus an `agentReason`. A set in which every permission
is `read` renders the see/act distinction as two identical labels. The concept
also gives up its only needs-attention service, so "quiet" (§6) and "needs-you"
collapse into the same picture and the state-coverage rulebook's most visible
distinction is never drawn. This is a set-selection error, not a wording one.

**Fix.** Compose `front-door` in place of one of the two `unavailable`/empty
services — keeping `notes-sync` is enough for the important
`sync.provider-reachable` (unavailable) vs `sync.local-pending` (3 notes, live)
split that §5 already gets right. Then bind two specific fixture rows to two
specific rules: `frontdoor.guest-code-used` → the named disabled control carrying
`agentReason` verbatim, and `frontdoor.back-door` → the Project Home handoff via
the fixture's `crossViewExample` action `lock-back-door`, whose
`resultingState` (`locked`, `front-door.state: healthy`, `attentionCount: 0`)
updates bay, Home and agent-facts in one commit.

## 2. Requirement 1 is unpinned again: no two-at-start, no named third service

**Wrong.** The brief requires the prototype to start with 2 connected services and
add a 3rd through *discover → credential reference → safe test → preview of the
facts/capabilities it offers → choose what to publish → it appears*. §2 names
three services as the initial cabinet and ends the intake with the unlabelled
"add the service bay"; nothing says which service arrives, and nothing says what
the "preview of the facts/capabilities it offers" step displays.

**Why it matters.** The build contract is a working prototype demonstrating
requirements 1–6 with real JS. An unpinned Connect lifecycle is the buildable
equivalent of a bullet — the author can ship any flow and still call it this
design. The preview step in particular is the only place requirement 1's "choose
what to publish" becomes a real decision, and without pinned fixture fact IDs it
becomes a decorative wizard.

**Fix.** Pin the whole arc in §2: start with `media-shelf` + `front-door`; the
wizard adds `notes-sync`, whose preview lists `sync.provider-reachable`,
`sync.local-pending` and `sync.last-synced` with their states and the person
ticking which to publish; the credential step accepts a *reference* only and says
in words why a value is refused. Say which steps have visible running/failed
states (`od-pack/state-coverage.md`, pattern 2 in design-research).

## 3. The signature element is motion, and motion is removed by the floor

**Wrong.** §4 says "A tactile press/release may confirm opening", and §9 makes the
signature "a physical-feeling inset service screen nested within a persistent
Worlds cabinet". §6 says "reduced motion strips drawer travel", and §8 assigns the
skeuomorphic family with "raised/pressed affordance". The design system card
(`od-pack/ds-skeumorphism.md`) is explicitly "playful" real-world mimicry, while
the design reads itself as "a calm personal appliance, with low variance".

**Why it matters.** Two rulebooks are in tension and the design resolves it in
favour of the metaphor. `od-pack/animation-discipline.md` allows animation for
real reorientation and sets 150 ms as the ceiling; the brief's exclude list forbids
"motion on by default", and the floors require `prefers-reduced-motion` honoured.
A signature whose recognition value is a press/release affordance disappears for
exactly the users the floor protects, leaving an inset rectangle. That is the
"decorative" case the animation rulebook says not to animate for. §8's own risk
note ("suggest affordances the prototype cannot honor") concedes the frame.

**Fix.** Make the static part the signature — the inset screen nested in a cabinet
bay, with the owner identity and fallback link permanently visible in the frame
edge — and drop the press/release as a recognition device. If a press state
survives, pin it as ≤150 ms, disabled entirely under `prefers-reduced-motion`, and
never as the only indicator of state. Reconcile "playful" skeuomorphism with the
appliance reading in one written sentence rather than leaving both.

## 4. The cabinet is spatially adjacent to round 1's `foldout-atlas`

**Wrong.** §9's reference lock is Teenage Engineering OP-1 and Apple Home, and §7
lists the deliberate differences — but not against the round-1 concepts, which the
brief's anti-repeat list puts in the same prohibition. Round 1's `foldout-atlas`
is: places on a map, evidence that "unfolds into the selected place", a field note
that opens inline, and a media app "docked as an app window within the atlas" with
an "Open in the owning app" link. c407's bays + pull-forward drawer + nested inset
service screen + fallback link is that composition with the map removed.

**Why it matters.** The brief permits an evolution of the author's *own* round-1
concept only when it becomes a real integrated shell; a copy of anyone else's is
not allowed. As written, nothing in the cabinet metaphor requires the integration
that round 1 lacked — a flat list of three rows with a detail pane would satisfy
most of §2–§6 identically. The metaphor is carrying no integration weight, which
is the owner's round-1 complaint restated.

**Fix.** Either commit to the difference in one sentence in §7 ("bays are
independent owners in a fixed reading order, not places on a map; nothing
unfolds, nothing routes between services, no route tracing") or change the
mechanic so the metaphor earns its place — e.g. the bay's drawer is where the
embedded frame *and* the shared fact card physically dock together, which is the
one-fact-many-places binding requirement 4 actually asks for. Also drop or keep
the OP-1 reference lock honestly: §7 already says the tactile cue adds recognition
"without implying mechanical control", which is a smaller claim than the lock in §9
makes.

## 5. The fallback link path is marked UNKNOWN though the fixture fixes it

**Wrong.** §4 says the fallback is "a visible owner link at
`https://example.invalid/...`; the final path is UNKNOWN". The fixture names every
fallback exactly: `media-shelf` → "Open Media app", `app://media`;
`notes-sync` → "Open Memory app", `app://memory`; `local-weather` → "Open provider
settings", `app://settings/providers`. `_meta.syntheticNote` states links use the
`app://` scheme only.

**Why it matters.** Requirement 3's fallback is a visible escape route from the
embedded frame; "path UNKNOWN" either means the fixture was not consulted or defers
the decision to build time, where it will be filled with a guessed host — the floor
forbids invented data. The design is also inconsistent with itself: §4 asserts
"the frame is sandboxed with no ambient access to Worlds credentials" as fact and
then says "the exact policy ... are UNKNOWN" in the same section.

**Fix.** Pin the three `owner.fallback.label` + `href` pairs verbatim, keep the
sandbox flags as the only UNKNOWN, and resolve the internal contradiction by
stating the intended boundary as a design rule ("the frame receives no Worlds
credential by design; whether the browser can enforce it is UNKNOWN") rather than
both claiming and disclaiming it.
---
## c616 — your DESIGN.md

# c616 — Service ledger

## 1. Metaphor in one line, and the main gesture.
Worlds is a clipped operations ledger: independent services file their own current facts into a shared index. The main gesture is **select a service row, then open its app in the work pane**; home, service, and agent facts remain linked views of the same observation.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Connect is a sequential rail: discover → enter a credential reference (never a secret value) → safe test → preview offered facts/capabilities → choose what to publish → service row appears. Three fixture services anchor the populated shell: `media-shelf` (Media Shelf, healthy), `front-door` (Front Door, needs_attention), and `nightly-backup` (Nightly Backup, stale). Each owns its records and actions. Worlds indexes published facts with source, observation time, and independent agent/human permissions. A service update rebinds the ledger row, Home attention summary, and agent-facts record from one fact identity; it does not copy authority.

## 3. Navigation between Worlds and a service (phone and desktop).
Desktop uses a compact left rail for Home, Connect, Memory, Settings and a dense service register beside the selected service pane. Service rows are links, not expandable command consoles. Phone collapses the rail into labeled top-level navigation and presents one service at a time; a persistent “Back to Home” control returns to the World index. Keyboard order follows navigation → main register → service pane; no hover-only row controls.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The selected service's own UI occupies a sharply bordered work pane. The frame is isolated from the shell and receives no Worlds credential by default; exact sandbox flags and cross-origin protocol are UNKNOWN. Shell navigation, service identity, state text, and Back remain outside the frame. If embedding is disabled or fails, show the fixture owner's fallback as an explicit link, such as `https://example.invalid/media`, labeled “Open in owning app”; actual URL mapping is UNKNOWN and must not be guessed. No Worlds decision is rendered as a service action.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
“Agent facts” is a readable ledger view over the same fact IDs used by Home and the service surface. Every row states source, checked/observed time, freshness (`live`, `stale`, `unavailable`, `unknown`), “Agent can see,” “Human can see,” and “Who can act.” For `frontdoor.back-door`, both can read; only the human can act, through Home. For `frontdoor.guest-code-used`, the agent sees no identity and the human can read it. Disabled permissions remain named with their reason. Any governed approval links to Project Home; other actions hand off to the owning app.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
Zero services shows the Connect lifecycle and states that no service is connected; zero providers remains useful with local Memory and no invented values. Quiet uses the fixture's Media Shelf healthy state. Needs-you elevates Front Door with “Needs a human” and its reason, while leaving the decision to Home. Nightly Backup retains its last facts marked stale. Garden Camera remains unknown with the fixture reason and no inferred frame. Disconnect removes the service row and embedded app, says which service left, and preserves the rest of Worlds; retention of historical facts is UNKNOWN. Loading, failure, and long-content edge states need explicit treatment in the later prototype. Labels accompany every color/state; default motion is none and reduced motion is honored.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's independently refreshed service views, Dashy's app opening in place and file-like configuration, HA's explicit state vocabulary, and MCP ext-apps' host-mediated requests. Deliberately, Worlds does not become a provider dashboard or action broker: rows summarize only published facts, embedded apps retain their own authority, and agent read access never implies act access. These mechanics are informed by the local research note; external source verification remains UNKNOWN.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `industrial-brutalist-ui` + `trading-terminal`. Use the skill's Swiss industrial print mode, not its CRT mode: strict square grid, ink-paper substrate, large editorial labels, compact mono metadata, and hard rules. Use the assigned system's flat, dark, tabular data grammar only where compatible with the brief; exact palette reconciliation is UNKNOWN and must respect AA, low glare, no pure-white large surfaces, and no neon. Apply anti-slop rules (no fake metrics or filler), color discipline (status text, not color alone), typography scale/tracking, state coverage, 44px controls, semantic order, and static-by-default motion. The design's density is bounded to service rows and fact tables, not continuous terminal telemetry.

## 9. Signature element, hero thesis, reference lock.
Signature: a single **three-view fact line** that visibly connects a service row to Home attention and the agent ledger. Hero thesis: “A service owns its work; Worlds makes its facts findable.” Reference lock: Bloomberg Terminal's tabular scan discipline (mechanics only) and Home Assistant entity cards' explicit state subscriptions (mechanics only). The ledger is the characteristic artifact, not a decorative dashboard.

## 10. Risks and UNKNOWNs.
The trading-terminal system's pure-white text, neon data colors, density, and market metaphors conflict with Worlds' comfort and state rules; reconcile before implementation rather than copy literally. Real iframe capability, isolation policy, service manifest/API, Connect contract details, disconnect retention, provider discovery, and test semantics are UNKNOWN. The fixture defines sample values and permissions only; no other facts may be invented. This is a design plan, not a tested prototype.


## c616 — CRITIQUE.md

# CRITIQUE — c616 "Service ledger"

Critic engine: MiniMax-M3.1-Flash-Preview (not the author). Scope: `DESIGN.md`
for c616 against `docs/vnext/concept-brief-v2.md`, `docs/vnext/fixture/fixture.json`,
`docs/vnext/design-research.md` and `docs/vnext/od-pack/`. 5 gaps, most important
first. The design is not rewritten here.

## 1. Requirement 4 is asserted but never instantiated by a named fact or action

**Wrong.** §2 says "A service update rebinds the ledger row, Home attention
summary, and agent-facts record from one fact identity; it does not copy
authority", and §9 makes the signature "a three-view fact line that *visibly
connects* a service row to Home attention and the agent ledger". No fact ID, no
action, no before/after state is named anywhere in the document.

**Why it matters.** Requirement 4 is the reason round 2 exists, and its test is
causal, not visual: one change in one service updates home, that service's surface
and the agent-facts view **at once**, from one fact identity. "Visibly connects"
describes a drawn line or repeated row — three copies that happen to agree — which
is exactly the round-1 "concepts on a page" failure. The fixture hands this concept
the canonical case and c616, the one design that composes `front-door`, does not
name it: `crossViewExample` gives fact `frontdoor.back-door`, action
`lock-back-door`, and `resultingState` `{frontdoor.back-door: locked,
front-door.state: healthy, attentionCount: 0}`.

**Fix.** Name that dependency as the prototype's second working interaction, and
say in §2 that all three views read one store keyed by fact ID (the "one render
from one state object" pattern round 1's desk concept used). Replace the signature
wording: the signature is *one fact ID rendered in three views and changed in one
commit*, not a visual connector. Note that the action is a mock that records a mock
review only — it does not lock anything.

## 2. Requirement 1 has no start state and no named third service

**Wrong.** The brief requires starting with 2 connected services and adding a 3rd
through *discover → credential reference (never a secret value) → safe test →
preview of the facts/capabilities it offers → choose what to publish*. §2 anchors
**three** services at once (`media-shelf`, `front-door`, `nightly-backup`) and
ends the rail with the unlabelled "service row appears". Which service arrives is
unstated, and the "preview of the facts/capabilities it offers" step — the only
place a person actually chooses what Worlds publishes — is never described.

**Why it matters.** Requirement 1 is the first thing the prototype has to prove;
as written it is a diagram of six labels with no entry or exit condition. "Sequential
rail" also fixes nothing: no step has a named state, no step has a failure, and
`od-pack/state-coverage.md` (dirty/submitted-pending on the credential field,
error on the safe test) goes unclaimed for the one form the concept contains.

**Fix.** Pin it: start with `media-shelf` + `front-door`; the rail adds
`nightly-backup`, whose preview lists `backup.last-success`, `backup.last-attempt`
and `backup.size` with their stale states and lets the person choose which to
publish. State the credential step as a reference-only field with visible
submitted-pending and validation-on-blur behaviour, and the safe test as a named
action with run/done/failed states (design-research pattern 2).

## 3. §6 writes state copy for a service the concept never composes

**Wrong.** §2 anchors `media-shelf`, `front-door` and `nightly-backup`. §6 then
spends a paragraph on "Garden Camera remains unknown with the fixture reason and no
inferred frame" — Garden Camera is not in the composed set and has no row, register
entry or index card. Meanwhile the composed set covers healthy, needs_attention and
stale but **no** unknown, unavailable or not_configured service, so `local-weather`
(facts: [], `emptyReason`), `notes-sync` (`sync.provider-reachable` unavailable vs
`sync.local-pending` live) and `studio-printer` (`printer.queue` live while
`printer.status` unavailable) never appear — and those are the cases where a naive
build collapses two states into one, which is what the fixture's own
`statesVocabulary.rule` forbids ("a blank value is never a state").

**Why it matters.** §6 is the section that proves honest state handling, and half of
it describes an artifact the prototype will not have. The set, not the prose, is
what decides which states can be screenshotted.

**Fix.** Make `garden-camera` the service added by the Connect rail (fix #2) or the
fourth composed row; it then carries the unknown state, `unknownReason`, and the
`camera.human-detected-today` `agent: none` rule. If the set stays at three, delete
the Garden Camera sentence from §6 and confine §6 to states the composed services
can actually render.

## 4. §5's permission statements drift from the fixture

**Wrong.** "For `frontdoor.back-door`, both can read; only the human can act" — the
fixture records `agent: read, human: act` with `humanReason: "Locking the door
changes the external system and is the person's decision."` Human `read` is not in
that row. And "Disabled permissions remain named with its reason" is stated as a
general policy; the fixture's `permissionVocabulary.rule` is stricter — a `none`
permission must render as **a named, disabled control carrying its own reason**
verbatim (here, `frontdoor.guest-code-used`'s `agentReason`), never hidden.

**Why it matters.** Requirement 4 asks the agent-facts view to show who can see and
who can act *separately*; inferring `read` from `act` (or vice versa) collapses the
distinction the requirement exists to make. The `agentReason` text is also the
fixture's proof that the agent sees *that* a guest code was used and not *who* —
a sentence worth rendering, not paraphrasing.

**Fix.** Transcribe the permissions column for each composed fact from the fixture
verbatim — `frontdoor.back-door` (`agent: read`, `human: act` + `humanReason`),
`frontdoor.guest-code-used` (`agent: none` + `agentReason`, `human: read`),
`frontdoor.unlocked-for` (both `read`) — and specify the agent-facts row for a
`none` fact as a disabled, focusable control whose accessible name includes the
reason text, so keyboard and screen-reader users get it too.

## 5. The ledger lands in the CRT/terminal family the brief excludes, and the assigned system is left half-applied

**Wrong.** §8 takes `industrial-brutalist-ui` + `trading-terminal` and admits
"the trading-terminal system's pure-white text, neon data colors, density, and
market metaphors conflict with Worlds' comfort and state rules", then keeps "the
assigned system's flat, dark, tabular data grammar **only where compatible** with
the brief; exact palette reconciliation is UNKNOWN". §10 repeats the deferral.
Meanwhile §9's reference lock is "Bloomberg Terminal's tabular scan discipline"
plus HA cards — Bloomberg Terminal is the assigned system's own prior art
(`od-pack/ds-trading-terminal.md` lists it), and HA cards are already in §7's
borrow list.

**Why it matters.** Two problems at once. First, the brief's anti-repeat list names
"Pip-Boy/CRT terminals" in the past library, and a dark dense mono register of rows
with tabular scan discipline is that look; the nearest round-1 neighbour is `the-desk`
(plain text, rows, tallies). A dark ledger is the least distinctive available reading
of this brief. Second, "only where compatible" and "reconciliation is UNKNOWN" is
lever 1 deferred — the design's job is to bind to the assigned system, not to
schedule the binding. And a reference lock drawn from the design system it was
already given, plus a mechanic already borrowed in §7, constrains nothing: a lock
has to name designs that could have been used instead.

**Fix.** Decide the conflict now and write the decision: keep the skill's Swiss
industrial *print* mode (ink-paper substrate, hard grid, compact mono metadata)
and drop the terminal's dark surface, neon data colours and buy/sell semantics
outright — the fixture's status words and `unknown`/`stale` states read better on
paper anyway, and it satisfies the low-glare floor without reconciliation. Then
replace §9's lock with one or two named designs that carry mechanics Worlds still
needs and that appear nowhere else in the document, and say in §7 what the ledger
is *not* versus `the-desk` (no sheet, no caret, no shared tally line — the register
is a table of owners).