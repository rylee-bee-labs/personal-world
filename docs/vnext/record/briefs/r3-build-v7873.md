# Worlds vNext real round: revise and build concept v7873

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md` AND `docs/vnext/concept-brief-v2.md` (all of v2 still applies), `docs/vnext/reference/direction-C-DESIGN.md` (the direction Rylee chose), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for concept v7873: skill file `docs/vnext/od-pack/skill-field-notes-editorial-template.md` and design system `docs/vnext/od-pack/ds-contemporary.md`.

1. Write `docs/vnext/round3/v7873/DESIGN.md`: the design below revised to fix every critique gap, with a short "Critique fixes" section.
2. Then build `docs/vnext/round3/v7873/index.html` FROM it. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.
Write only those two files, both before finishing. Do not push.

---
## Your DESIGN.md

# v7873 — "The Pass" (a service counter)

MOCKUP. DESIGN.md written before any code, per the round-2 guide ("never one-shot": this file → critique by another engine, ≤5 gaps → fix → build → screenshot check).

## 1. Metaphor in one line, and the main gesture

Worlds is the pass of a small house workshop: your services stand at the counter, each owning its own work, and the only thing Worlds actually makes is the **stub** — a tear-off slip that says what you asked the owning app to do, who can do it, and when. Main gesture: **read the slip, then push it through the hatch.** A decision is never made here; it is written on a stub and handed to Home, Media, Studio, Memory or Settings, and the stub is kept.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow

The shell opens with exactly two services from `fixture.json`: **Media Shelf** (`healthy`, quiet) and **Front Door** (`needs_attention`, reason verbatim: "A decision is required; the local app will not act on its own."). `#add-service` runs the six-step lifecycle for the third service, **Studio Printer** (`unavailable`, `retry.allowed: "human"`), and Discover can later offer the remaining fixture services as themselves — Nightly Backup (`stale`), Garden Camera (`unknown`, its `unknownReason` verbatim), Notes Sync (`unavailable`, with `sync.local-pending` still `live`), Local Weather (`not_configured`, zero facts, its `emptyReason` verbatim). No id, name, state, value or link outside the fixture is invented.

Steps: **discover** → **credential reference** (a reference only; a pasted secret is refused in copy — "Worlds stores a credential reference, never the secret" — with untouched / dirty / submitted-pending / validate-on-blur states and input preserved on error) → **safe test** (run / running / passed / failed, with `state-coverage.md` retry discipline: immediate first retry, 2s then 4s, then "Retrying is done — open the owning app") → **preview** (fixture fact ids with their states, e.g. `printer.status` `unavailable`, `printer.queue` `live`) → **choose what to publish** (a checkbox per fact, stating what will be shared) → **publish** (the service takes its place at the counter). One store keyed by fact id feeds every view through a single `render()`, so Home, the service surface and Memory can never disagree.

## 3. Navigation between Worlds and a service (phone and desktop)

Four destinations — **Home / Connect / Memory / Settings** — kept from C because they are right for a daily app. Desktop: destinations in a left rail, every connected service as a slip (name, state word, age) in a counter column, the selected service in the main column, Memory in a right drawer. At 390px the destinations become a bottom bar and a slip pushes a full-screen sheet with Back. Focus moves to the service name on entry and returns to the invoking slip on Back. Source order is navigation → main → drawer; everything is keyboard-operable with a 2px `:focus-visible` ring at ≥3:1, 44px targets, no sideways scroll at 390px.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it)

The **hatch**: a flat, bounded inset with a permanent owner lip — service name, state word, owning app, and the fixture's fallback link verbatim ("Open Media app" `app://media`, "Open Home app" `app://home`, "Open Studio printers" `app://studio/printers`, "Open Memory app" `app://memory`, "Open Backup settings" `app://settings/backups`, "Open Home cameras" `app://home/cameras`, "Open provider settings" `app://settings/providers`). `app://` does not resolve in a browser, so the designed behaviour *is* the failure path: the hatch keeps the shell, says plainly that the embedded app could not be opened, and leaves the owner link as the way through. No credentials cross into the hatch by design; browser-enforced isolation, sandbox flags and app messaging are UNKNOWN. Approvals hand off to Project Home, everything else to the owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts)

One fact record, no merged columns: **source** (`local` / `external` / `none`), **freshness** ("checked 2s ago", recomputed from `checkedAt`, never from `ageSeconds`), **agent permission**, **human permission**, and then separately **who can act**. `frontdoor.back-door`: agent `read`, human `act`, reason verbatim. `frontdoor.guest-code-used`: agent `none` renders as a **named, focusable, disabled control** carrying the exact reason — "Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who." Same for `camera.human-detected-today`. Memory shows the same rows, so an agent and a person read one surface, not two copies.

## 6. States: empty/first run, quiet, needs-you, stale, unknown, disconnected

**First run**: zero services — a bare counter, one invitation to connect a first service, no invented readings. **Quiet**: Media Shelf, "Library reachable and indexing is current." **Needs-you**: Front Door, "Needs a human" plus its `attentionReason`; a stub is all Worlds can do. **Stale**: Nightly Backup and `sync.last-synced`; the age grows and is never quietly refreshed. **Unknown**: Garden Camera, its reason shown and its values `—`, never blank. **Not configured**: Local Weather, zero facts, its `emptyReason`. **Error**: Studio Printer and a failed safe test, each with cause, recovery and preserved input. **Disconnected**: `#disconnect` confirms by name, removes the hatch and slip, and keeps the stub that referred to it: "Front Door is disconnected — this stub can no longer be sent." Loading, empty-preview and long-copy states each carry recovery copy.

## 7. Borrowed, and deliberately different

Borrowed: Homarr's *one integration feeding many surfaces*, Dashy's *open the app in place*, Home Assistant cards' *literal state words*, MCP ext-apps' *contained UI and host permission boundary*. Deliberately different: the primitive is **the stub**, not the tile. There is no widget grid, no drag-to-arrange, no map, no route tracing, no unfolding place, and no topology — services are independent owners standing at a counter in a fixed reading order, and what connects them is a piece of paper that travelled and came back.

## 8. OpenDesign starting point and guide rules applied

Starting point: `skill-field-notes-editorial-template` + `ds-contemporary`. From the skill I take the *editorial frame language* — paper-white surface, strict hierarchy, ruled sections, rounded soft cards, honest `—` placeholders, count-up on key numbers — and drop its serif display: the stack rule bans external fonts, the assigned system binds Jost (sans) for display, and anti-slop rule 4 penalises sans display only when the seed binds a serif, so a geometric-sans display with `ui-monospace` for figures is the correct resolution. From `ds-contemporary` the tokens come verbatim: `--accent: #C800DF`, `--secondary: #E60076`, `--surface: #FFFFFF`, `--text: #111827`, semantic colours as non-text markers only, bento rhythm, 150–250ms on `cubic-bezier(0.2, 0, 0, 1)`, `--accent` capped at two visible uses per screen (the selected slip, the one primary action). Contrast as gates, not hopes: `#C800DF` on `#FFFFFF` is 4.64:1 and passes for text; on the derived 3% ink wash (`rgba(17,24,39,0.03)`, derived from `--text`, not an invented hue) it is 4.45:1, so **accent text lives only on white** and the wash carries `--text`. `#16A34A` on white is 3.30:1, so semantics are markers and rules, never body text — the state is always a word. Type: one 1.25 scale of seven sizes (44 / 32 / 25 / 20 / 16 / 14 / 12), body 16 at 1.55. Whimsy is capped at three moments — the bell ding when a stub lands, the count-up, one small inline-SVG accent — and warmth lives in copy and the stub's typography. `prefers-reduced-motion` strips transforms and keeps opacity crossfades.

## 9. Signature element, hero thesis, reference lock

Signature: **the stub** — a slip with a torn edge, one typed line, and the two permission words on it, kept in the drawer after the owning app has done its part. The bell ding is its punctuation, not a second signature. Hero thesis: *"Worlds keeps the stub; the owning app keeps the decision."* Reference lock: (1) the **workshop order docket** — the paper slip that travels with a job and is signed for; (2) the **handover book** in a night-shift kitchen, where each shift writes down what changed. Mechanics only, no palettes, no textures, no distressed paper filters.

## 10. Risks and UNKNOWNs

A stub can be misread as "Worlds recorded a decision" — it records a *request*, and the copy must keep saying so. Real embed support, browser-enforced isolation, the Connect protocol and disconnect history are all UNKNOWN. The feed can only re-anchor `checkedAt` where the fixture source is `local` and the state is `live`; a service whose source is `none` (Garden Camera) or already `stale` (Notes Sync, Nightly Backup) never heals, because inventing a recovery reading would be a fixture lie — so "a service moving to stale" is expressed as **age crossing its own threshold**, never as a fabricated transition. Fixture data is synthetic and stays labelled MOCKUP. Long service names and a 200-character `lastError` must not break the slip. Whether a paper metaphor survives contact with a settings screen is UNKNOWN; Settings is deliberately plain.

## 11. How it differs from C, and what it keeps

**Keeps from C:** the four-destination shell, the per-service own-app frame with a permanent owner fallback, one fact store driving Home + service surface + Memory, source/freshness/who-sees/who-act on every row, the named-but-disabled agent permission with its verbatim reason, and the refusal to decide anything in Worlds. **Differs:** C files services into fixed *bays* and its signature is the docked frame-plus-fact card; here the primitive is the *stub* — the record of a request that leaves — so a service's defining artifact is what it was asked to do, not where it sits. Three concrete moves: (a) the hatch is a **pass-through window with a permanent lip** whose designed path is the embed-failure path; (b) `#disconnect` and `#simulate-change` commit like everything else and both leave a stub behind, where C leaves no trace; (c) every broken fixture service (`unknown`, `unavailable`, `stale`, `not_configured`) is reachable through Connect and keeps its own honest reading — C surfaces none of the first or last. Same data, different job.

## 12. Desktop and mobile layouts (both designed on purpose)

**Desktop, 1440px.** A 248px rail (destinations, live toggle, MOCKUP label), a 320px counter column of slips, a fluid main column, a 360px drawer. Main column order: service header (name, state word, age, owner app, `#disconnect`) → the hatch → the label card with its facts → the stub rack. `#disconnect` is visible in the header of the selected service on first paint, with no opening, and confirms in a dialog whose confirm button reads "Disconnect". `#add-service` sits at the foot of the counter column; `#simulate-change` and `#live-toggle` sit at the top of the rail, always reachable. Tablet (768px) drops the drawer into a section under the hatch.

**Mobile, 390px.** Not a squeezed desktop: no rail, no drawer, no third column. A bottom bar carries the four destinations at 44px; a sticky top strip carries the selected service's name, state word and `#disconnect`. The counter is one slip per row — name and state word, plus a mono age. Selecting a slip pushes a full-screen sheet with its own reading order: **identity and state → what needs you and the stub → the hatch (capped at 40vh, with "Expand the app" to take the whole screen) → the fact label card**, because on a phone the decisions and the text matter more than the embedded app, and the app stays one tap away. `#add-service` is a row at the foot of the counter and a destination in the bottom bar; `#simulate-change` and `#live-toggle` live in the top strip on mobile, so all four checker ids are on the first screen at both widths. Connect runs as a full-screen stepped flow with real `<button>`s per step.

**The feed.** A 4s tick recomputes every age from `checkedAt` and re-anchors it where the fixture source is `local` and live, so "checked 2s ago" becomes "12s ago" and settles back without a reload. `#live-toggle` is ON by default and genuinely pauses and resumes the tick; reduced motion is a separate switch and changes only how a change settles (instant, no transform, no count-up), never whether the data moves.


---
## CRITIQUE.md (another engine)

# Critique of v7873 "The Pass" DESIGN.md

Reviewer: another author, not grading generously. Five gaps, most important first. Section numbers refer to the v7873 DESIGN.md; "brief v3" and "brief v2" are the concept briefs.

## 1. `#simulate-change` is never defined, and the stub model contradicts the fixture's cross-view change

**Missing or wrong.** Brief v3 requires `#simulate-change` to "trigger one fixture change that updates Home, the service surface and the agent-facts view at once". Brief v2 requirement 4 asks the same. The only fixture change that does this is `crossViewExample`: `lock-back-door` takes `frontdoor.back-door` to `locked`, `front-door.state` to `healthy`, and `attentionCount` to 0.

DESIGN.md names `#simulate-change` only in §11(b) and §12, as a control that "leaves a stub". §2 and §5 never say which change it applies or what each of the three views shows afterwards.

§1 and §10 say a stub records a request, not a decision, and that Worlds decides nothing. If the stub is only a request, the door is still unlocked and Front Door still needs a human. If the three views flip to `locked` and `healthy`, Worlds has asserted a decision it says it cannot make. C resolved this explicitly as "mock review/handoff only". v7873 leaves it open.

**Why it matters.** This is the checker's core test and the integration spine (requirement 4). It is also the one place where the stub metaphor has to carry weight. Left unresolved, the build will either break the honesty rule or fail the checker.

**Fix.** Add a §2 paragraph with three parts:
- Pin `#simulate-change` to `lock-back-door`.
- State the simulated sequence: the stub goes to "sent to Home", then a labelled MOCKUP "owner answered" event lands. Only then do `frontdoor.back-door` → `locked`, Front Door → `healthy` and attention count → 0 change.
- List the exact text each of Home, the service surface and the Memory/facts rows shows, so the update is one store write.

Also define how the stub is closed, with "answered by the owning app (simulated)". As written, §9 says the stub is kept "after the owning app has done its part", but nothing in the mock plays the owning app.

## 2. The honesty rules in the feed (§10, §12) are self-contradictory and rest on invented thresholds

**Missing or wrong.**
- §12 says the tick "re-anchors" `checkedAt` for local live services, so "checked 2s ago" becomes "12s ago" and "settles back". That manufactures fresh observations the fixture never made. It conflicts with §5, "recomputed from `checkedAt`", and with the fixture rule that a viewer recomputes age and nothing is invented.
- §10 says a service "moving to stale" is "age crossing its own threshold". The fixture has no thresholds. Nightly Backup mentions only an unquantified "configured daily window". An invented threshold is the fabricated transition §10 says it avoids.
- The only live-to-stale candidates are the local-live services (Media Shelf, Front Door, Studio Printer). Re-anchoring keeps them forever fresh, so brief v3's "occasionally moves a service to stale" can never happen on the feed.
- The fixture ages (28s, 24s, 106s) disagree with the "2s" in §5's example.

**Why it matters.** The brief's honesty floor is "nothing presented as a real reading". A feed that re-stamps `checkedAt` presents a fake heartbeat as real. It also makes the reactivity requirement impossible to meet truthfully.

**Fix.**
- Let ages only grow from the fixture `checkedAt`. Never re-stamp.
- Make the staleness change an explicit, labelled MOCKUP event, scripted as part of the feed (for example, "Media Shelf's check is overdue" after N ticks) with a stated MOCKUP threshold.
- If a "heartbeat" is wanted, show it as a labelled simulated check ("simulated check landed"), not as the owner's reading.
- Say what the UI shows while paused: ages freeze with a visible "paused at hh:mm:ss" label, and the first resume tick is stated.

## 3. The daily-use shell is thinner than the brief asks, and the 390px layout fails the "first screen" contract

**Missing or wrong.**
- §12 says all four checker ids are on the first screen at both widths. At 390px the first screen is the counter list. A service is selected only after a slip pushes its sheet, yet `#disconnect` lives in the sticky strip "of the selected service". Either no service is selected on load, or the list is hidden behind a detail view. Brief v3 requires `#disconnect` visible for the currently selected service on the first screen.
- The top strip at 390px holds name, state word, `#disconnect`, `#simulate-change` and `#live-toggle`. At 44px targets that is about 5 controls in 390px, with no layout for it. This is the squeezed-desktop failure v3 forbids.
- At 1440px, 248 + 320 + 360 leaves roughly 500px for a main column holding the hatch, label card and stub rack. The embedded app becomes the smallest thing on screen.
- Memory is both a destination and a right drawer, and nothing says what the Memory destination shows that the drawer does not.
- Settings is "deliberately plain" with no contents. Settings should list at least the feed pause, the reduced-motion override, sound, and credential references.
- First run is "a bare counter, one invitation". Missing are the empty feed, the zero-providers copy, and the exit path into Connect.
- No shell-level error is defined. The feed failing, the hatch failing to load, and offline are all absent, apart from the `app://` failure path.

**Why it matters.** This is requirement 1 of brief v3, a real application used every day. At the moment it reads as a counter plus a metaphor, not a shell with a working Settings, Memory, first-run and error path.

**Fix.**
- Preselect Front Door on load at 390px. Open straight into its sheet, or pin a compact "selected service" bar above the list with `#disconnect` visible.
- Move `#simulate-change` and `#live-toggle` into one 44px "Feed" row under the strip, and give the strip a layout spec.
- Collapse the desktop drawer into the Memory destination, or state the difference between them.
- Write the Settings sections as a list, and add one shell-level error state with copy and a recovery action.

## 4. Whimsy and warmth are specified as a cap, not as content; one of the three moments is audio

**Missing or wrong.**
- §8 caps whimsy at three moments: a "bell ding", a count-up, and "one small inline-SVG accent". It then says warmth "lives in copy and the stub's typography" without giving any copy. There is no micro-copy and no character, and the SVG accent has no subject.
- The bell sound has no mute or setting. Sound on a state change in a daily-use app is an annoyance. It is also absent from the reduced-motion treatment, and nowhere is it tied to `prefers-reduced-motion` or to the reader's choice.
- A count-up on "key numbers" animates fixture values toward their real values. On a status surface this reads as a changing reading, which clashes with the honesty rule. Brief v3 also asks for a satisfying "connected" moment. §2 never gives the Connect publish step one, and the ding fires only when a stub lands.
- The one piece of copy that is quoted, the disconnect line "Front Door is disconnected — this stub can no longer be sent", is cold, not warm.

**Why it matters.** Brief v3 item 2 says "whimsy and warmth laced in", with specific, delightful details: kind micro-copy, a drawn accent, a gentle motion on state change, a satisfying "connected" moment. A cap and a reference to "copy" does not tell the builder what to make. The most likely build is a plain magenta-on-white app with one sound.

**Fix.** Add a short copy sheet to DESIGN.md for each of:
- the first-run invitation
- Connect publish success (the connected moment, for example the new slip being stamped and settling into the counter)
- stub sent, and stub landed
- an error line
- the disconnect line

Name the SVG accent, for example a small hatch bell or a stamp, and say where it appears. Make the ding opt-in (off by default, a Settings switch). Drop the count-up, or limit it to counts that actually change (attention count).

## 5. Difference from C and use of the OpenDesign sources are thinner than claimed

**Missing or wrong.**
- Structure: v7873's shell (four destinations, rail, service list, selected main, per-service frame with permanent owner lip, one fact store, same disabled-agent control) is C's structure renamed. A "counter column of slips" is C's register of bays. The stub is the one real difference.
- The stub's defining behaviour in v7873 is "a request that leaves", and C's handoff already does this. C routes `lock-back-door` to Project Home and records a mock review. v7873 does not show what is different about the stub beyond a torn edge and a ledger.
- §11(b) says C "leaves no trace" on disconnect. C leaves historical retention UNKNOWN rather than claiming no trace, so the contrast is overstated. Claim (c) is true but is about which fixture services are reachable, not about design.
- OpenDesign: `ds-contemporary` binds Jost for primary and display and Overpass Mono for mono. Neither can load under the no-external-fonts rule. §8 picks "geometric-sans display" and `ui-monospace` without a fallback stack, so rendering is system-dependent and Jost is not delivered. §8 should say this plainly.
- The "editorial frame language" from the field-notes skill is mostly dropped (its serif, its count-up). What remains is a paper-white card layout, which is not a distinctive use of the skill.
- The torn edge on the stub risks drifting toward a textured paper look. The reference lock says "no textures", but no rule says how the edge is drawn (for example a CSS clip, not an image).

**Why it matters.** Brief v3 says "use C as the starting direction, do not clone it", and asks for use of the assigned skill and design system. A reviewer comparing the two screens would see the same shell with different nouns.

**Fix.**
- Make the stub the main surface, not a list item. For example, each service's main column leads with its open stub and its history, so the service's identity is what it was asked to do, as §11 claims. Then back that with a concrete screen description.
- Correct the C comparison to what C actually says.
- State the font stack as `Jost` unavailable → `system-ui` with a metric-compatible fallback, and say the mismatch is accepted.
- Spell out the torn edge as a flat CSS clip-path with no image or texture.

