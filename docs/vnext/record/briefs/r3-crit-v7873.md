# Worlds vNext real round: critique concept v7873's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v7873/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v7873

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

