# Worlds vNext round 2, step 2: critique three DESIGN.md files (another author's)

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- Never one-shot: every design gets a critique by a different engine before it is built. You are that critic.

Read `docs/vnext/concept-brief-v2.md` and `docs/vnext/fixture/fixture.json`. Below are three DESIGN.md files by another author. For EACH, write `docs/vnext/round2/<concept-id>/CRITIQUE.md` with AT MOST 5 gaps, most important first. Each gap: what is missing or wrong against the brief (cite the requirement number or floor), why it matters, and the concrete fix. Focus on integration (plug-in live, composition, embedded frame, one fact many places, governed handoff, disconnect), honesty (stale/unknown, no invented data), uniqueness vs the anti-repeat list, and whether the OD skill/design system was really used. Do not rewrite the design; do not grade generously; if a design has fewer than 5 real gaps, list fewer. Write only the three CRITIQUE.md files. Do not push.


---
## DESIGN.md for c258

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

---
## DESIGN.md for c407

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

---
## DESIGN.md for c616

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
