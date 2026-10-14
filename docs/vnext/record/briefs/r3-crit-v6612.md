# Worlds vNext real round: critique concept v6612's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v6612/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v6612

# v6612 — The Welcome Shelf

## 1. Metaphor in one line, and the main gesture.
Worlds is a small, well-kept home for independent services: scan what needs you, then open the named service beside the facts it shares. Main gesture: select a service and move between its own workspace and its shared facts. A tiny inline SVG house-wren perches beside the greeting; it is decoration, never status.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start with exactly Media Shelf (`media-shelf`, `healthy`) and Front Door (`front-door`, `needs_attention`). `#add-service` opens a six-step Connect flow for Notes Sync (`notes-sync`, `unavailable`): discover → enter a credential reference only → safe test → preview the three fixture facts → choose which facts to publish → publish. Secret values are rejected; no provider call occurs in this mockup. Each service owns its app, actions, and data; a shared local store keyed by fixture fact ID feeds Home, its service screen, and Agent facts. `#simulate-change` applies only the fixture’s `lock-back-door` example: Back door becomes `locked`, Front Door becomes `healthy`, and attention count becomes zero in all three views. This is a mock handoff, not a real lock command.

## 3. Navigation between Worlds and a service (phone and desktop).
Persistent shell navigation is Home, Connect, Memory, Settings. A service’s screen is inside Worlds, with its owner and “Open [owner] app” fallback visible. Navigation, Back, and status remain Worlds-owned around the embedded service. Keyboard focus follows semantic source order; service selection moves focus to its heading and Back restores focus to the invoking control.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
Show the owner app in a clearly titled, bounded app frame beside a Worlds-owned fact panel. Keep the owner label, state, and fixture fallback link visible: Media Shelf → “Open Media app” (`app://media`); Front Door → “Open Home app” (`app://home`); Notes Sync → “Open Memory app” (`app://memory`). No credential is passed into the frame. Actual embed protocol, browser isolation flags, and messaging are UNKNOWN. If embedding fails, retain the facts and explain the failure beside the owner link. Approvals hand off to Project Home; other actions go to their owner.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Home cards, service facts, and Agent facts read the same records. Show value, source, observed/checked time, freshness as words, and separate “Agent can see,” “Human can see,” and “Who can act” fields. Preserve independent permissions: `frontdoor.back-door` is Agent read / Human act; `frontdoor.unlocked-for` is read by both; `frontdoor.guest-code-used` is Human read and Agent none, with its fixture reason shown in a named disabled control. Notes Sync’s unavailable provider fact does not erase its live local pending count or stale last-sync fact.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
First run shows an inviting empty shelf, local Worlds/Memory baseline, and one “Connect a service” action; no invented readings. Quiet uses Media Shelf’s exact `healthy` label. Needs-you uses Front Door’s “Needs a human” state and fixture explanation. Include Nightly Backup (`stale`), Garden Camera (`unknown`), Studio Printer (`unavailable`), Notes Sync (`unavailable`), and Local Weather (`not_configured`) when browsing the fixture; name the last observation or reason and what still works. A failed test identifies Notes Sync and offers retry. Disconnect says what service will be removed and leaves the remaining shell usable; retention history is UNKNOWN. State words and recovery copy never rely on color.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr’s integration reused across views, Dashy’s in-place app opening, Home Assistant cards’ shared state, and MCP ext-apps’ contained UI boundary. Unlike a widget dashboard or tool host, this is a personal front door: the owner is named, facts have explicit provenance and separate permissions, and consequential choices return to the owning app. No provider becomes Worlds’ product model.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `skill-ui-ux-pro-max.md` and `ds-duolingo.md`. The skill file is catalog-only; its full upstream search workflow is not bundled, so no unavailable pattern library is claimed. From Duolingo, keep friendly rounded headings, a restrained leaf-green action cue, and a single warm wren accent; discard high-saturation coverage, chunky button shadows, oversized lesson type, and gamification. Use calm neutral surfaces, readable sans-serif type, and consistent 4/8/12/16/24/32 spacing. Apply the guide’s one-job hero (the selected service with its live facts), one signature element (the wren beside a clear “All yours / Needs a human” greeting), named reference mechanics rather than copied palettes, real fixture copy, explicit state coverage, and no gradients, emoji icons, or invented metrics. Keep visible MOCKUP labeling.

## 9. Signature element, hero thesis, reference lock.
Signature: a tiny line-drawn wren offers one kind, contextual sentence beside the service summary; it never animates as telemetry. Hero thesis: “Your services keep their owners; Worlds helps their facts make sense together.” Reference lock: Apple Home’s accessory-to-owner handoff and Homarr’s integration feeding multiple surfaces; borrow mechanics, not visual language.

## 10. Risks and UNKNOWNs.
The Connect API, safe-test semantics, service embed protocol/isolation, real action authorization, and disconnect history are UNKNOWN. All values are synthetic fixture data and the UI remains MOCKUP. Validate actual contrast pairs, mobile safe areas, keyboard focus, and 200% reflow in implementation. Live freshness is simulated and must be labeled as such, never presented as a real observation.

## 11. How it differs from C, and what it keeps
C’s service cabinet pairs a fixed owner bay with a docked app and shared fact card. This concept keeps the persistent Home / Connect / Memory / Settings shell, clear ownership, honest fixture states, and adjacent owner app plus shared facts, while shifting the main gesture to a welcoming daily front door: a concise attention-first home leads into full service workspaces, with one restrained wren detail supplying warmth. It does not reproduce C’s cabinet/bay metaphor or its fixed register as the organizing visual.

## 12. Desktop and mobile layouts
**Desktop, 1440px:** persistent left navigation rail; centered content column with a first-day/attention summary and service list; selected service opens a two-column workspace with embedded owner app and shared facts side by side. Settings is a real shell destination, with live-feed pause/resume visible in its status controls and on the home freshness line. **Mobile, 390px:** compact top identity and a fixed, safe-area-aware bottom bar for Home, Connect, Memory, Settings; one-column reading order is greeting/attention, service summary, owner app, then facts and actions. Selecting a service opens its own full-width screen with an explicit Back control. Connect is a sequential full-screen step flow. Keep `#add-service`, `#disconnect` for the selected service, `#simulate-change`, and `#live-toggle` visible on the first Home screen; disconnect opens a confirmation whose action is labeled “Disconnect.” Targets are at least 44px, no sideways scrolling, focus is visible. `#live-toggle` is ON by default and pauses/resumes simulated freshness updates in every view. State changes settle with a brief 150ms opacity/color cue and a text update; reduced-motion removes transform effects and keeps all state legible without animation.

