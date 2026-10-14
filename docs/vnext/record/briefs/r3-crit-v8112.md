# Worlds vNext real round: critique concept v8112's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v8112/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v8112

# v8112 — Clay tray (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing here is a real reading. A visible "MOCKUP" label sits in the shell header at every width.

## 1. Metaphor in one line, and the main gesture.
Worlds is a tray of soft clay pebbles, one per service, each with a small face and a plain state word; the selected pebble settles forward and docks its owning app beside its fact card. Main gesture: **press a pebble to settle it, then read its app and facts together.** The face is an accent; the state word is always the truth.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
First screen has exactly `media-shelf` (Healthy) and `front-door` (Needs a human), with Front Door selected because it needs the person. `#add-service` starts Connect for `notes-sync` (Unavailable): (1) Discover (shows owner Memory); (2) Credential reference (reference only; a pasted secret is refused with "Worlds stores a credential reference, never the secret"); (3) Safe test (idle/running/failed/passed; first run fails with the fixture's "provider timed out" and offers "Check provider now" or "Skip and publish local facts only"); (4) Preview of `sync.provider-reachable` (unavailable), `sync.local-pending` (3 notes, live), `sync.last-synced` (stale); (5) Choose what to publish (one checkbox per fact id, plus a sentence on what is shared); (6) Publish, where the pebble arrives on the tray with a short settle and the words "Notes Sync is connected. Its provider is still unavailable; your 3 local notes are safe." No provider call occurs.
One in-memory store keyed by fact id feeds Home, the service surface and the Agent facts view. `#simulate-change` applies `lock-back-door` to `frontdoor.back-door` (from `crossViewExample`): the fact becomes `locked`, Front Door becomes Healthy, attention count 0, and all three views change in the same update. Worlds locks nothing; this records a mock hand-off. `#disconnect` is on the first screen, in the selected service's header; it opens a confirm naming the service with a button labelled "Disconnect".

## 3. Navigation between Worlds and a service (phone and desktop).
Four persistent destinations: Home, Connect, Memory, Settings (Settings holds the live-feed default, reduced-motion note, credential-reference list and provider settings). Desktop: left rail. Phone: bottom tab bar (four 44px+ targets). A service opens in place; phone gets a full screen with Back, focus moves to its heading and returns to the invoking pebble on Back. Source order: nav, main, facts.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The owning app sits in a clay-rimmed inset labelled with owner and state. Fallback is always visible under it with the fixture labels: Media Shelf "Open Media app" (`app://media`), Front Door "Open Home app" (`app://home`), Notes Sync "Open Memory app" (`app://memory`). The frame is meant to receive no Worlds credential; browser-enforced sandbox flags and messaging are UNKNOWN. If embedding fails, the shell, fact card and fallback link stay, with a one-line cause and "Try again". Approvals hand off to Project Home; other actions to the owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Agent facts rows show: fact, value, source (local/external/none), checked time and freshness word, "Agent can" and "Human can", and for any `none` a named disabled-but-focusable control with its fixture reason. `frontdoor.back-door`: agent read, human act (reason verbatim). `frontdoor.unlocked-for`: both read. `frontdoor.guest-code-used`: agent none ("Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who."), human read. Provider-unavailable and local-live facts of Notes Sync stay separate rows.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
- Zero services: an empty tray with one sleeping pebble outline, "Nothing plugged in yet. Connect your first service." and a button to `#add-service`. Zero providers: no readings invented.
- Quiet: Media Shelf, "Healthy", smiling face, a calm line.
- Needs-you: Front Door, "Needs a human" with the fixture reason; raised-hand face; listed first on Home.
- Stale: the live feed can age Media Shelf to stale (word "Stale", sleepy face, "Checked 2m ago"); Nightly Backup is available as a stale example.
- Unknown: never blank; "Unknown, nothing has answered".
- Error: failed safe test or dropped service has plain cause, recovery action, input preserved.
- Disconnected: "Front Door is disconnected. Its bay, app frame and 3 facts are gone from Worlds; the rest still works." Retention of history is UNKNOWN.
- Loading is a static skeleton; long copy wraps.
- Live feed: ON by default, ticks every 3s, recomputes "checked Ns ago" from `checkedAt` in every view, and `#live-toggle` pauses/resumes with a visible "Live" / "Paused" word. Reduced motion removes animation, never data updates. Actions give instant optimistic feedback ("Locking… recorded as a mock hand-off") then settle.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's one integration feeding many surfaces, Dashy's open-in-place, HA's literal state words, ext-apps' contained UI behind a host boundary. Different: no dashboard grid or topology; a flat tray with one selection and a fixed docked pair (app + facts).

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `skill-stitch-skill.md` (stitch-design-taste) + `ds-claymorphism.md`. From Stitch: a DESIGN.md of atmosphere, roles, banned list, "Daily App Balanced" density (5), motion low-to-fluid (4, only transform/opacity), one accent, no Inter, no emoji, no pure black, no neon glow, no filler copy, skeletal loaders, composed empty states, 44px targets, `clamp()` type, `100dvh`. From Clay: soft puffy radii (20/28px), 4/8/12/16/24/32 spacing, Poppins display / Montserrat body / JetBrains Mono for ids, Text `#1C398E`, Surface `#FFFFFF`, Primary `#3B82F6`, semantic success `#16A34A`, warning `#D97706`, danger `#DC2626`. Fonts load as local stacks only (no CDN); fallback system sans. Reconciliations: (a) Primary `#3B82F6` is 3.7:1 on white, so it is a non-text marker (focus ring, selection rim, progress) and never text; CTA is `#1C398E` fill with white label (about 11:1). (b) Stitch bans serifs in software and the 3-equal-card row; Clay's puffy depth is limited to pebbles and the frame rim, using one soft inset shadow tinted to the canvas (a pale blue-white `#EEF4FF`), never glow. (c) Max two accent uses per screen. Craft rules applied: anti-ai-slop, color, typography, laws-of-ux, state-coverage (all five states per surface), accessibility-baseline, animation-discipline (motion only for a state change or reorientation, 150-250ms, off under reduced motion). Status words and shapes (face + word + text) are never colour-only.

## 9. Signature element, hero thesis, reference lock.
Signature: **the pebble's face**, an inline-SVG face per state (smile, raised hand, sleepy, puzzled, blank for not configured) that changes with a small settle when a fact changes, always paired with its word. Hero thesis: "Every service gets a place, a face, and an honest word." Reference lock: Apple Home's accessory tiles that open to detail (mechanic only), and Duolingo-style small character feedback (tone only, no gamification). No palettes borrowed.

## 10. Risks and UNKNOWNs.
Embed support and browser-enforced isolation, Connect API, credential storage, and disconnect history are UNKNOWN. Faces could read as decoration or imply capability: the word always leads. Clay softness could blur hierarchy: borders are 2px `#1C398E` at low alpha on controls, with 3:1 checked on implemented pairs. Synthetic data must stay labelled MOCKUP.

## 11. How it differs from C, and what it keeps.
Keeps C's real shell (Home / Connect / Memory / Settings), owner-labelled services, the docked app-plus-fact-card, owner fallback links, fixture copy verbatim, the six-step Connect, and its honest state language. Differs: C's fixed register of square bays with a static bezel becomes a warm, soft tray of selectable pebbles with state faces and a settle-on-change moment; C is deliberately static, v8112 is reactive (3s feed, optimistic-then-settle actions, visible Live/Paused); kind micro-copy and a "connected" moment; a mobile-native pattern rather than C's shrunken register; a failing-test recovery path in Connect.

## 12. Desktop and mobile layouts.
**Desktop (1440):** three columns on a CSS grid. Left rail (240px): wordmark, MOCKUP label, four destinations, live toggle `#live-toggle` with the word "Live"/"Paused". Centre (flex): top "Today" sentence ("1 thing needs you."), the tray of pebbles (Media Shelf, Front Door) plus a dashed "Add a service" `#add-service` pebble, then below it the selected service's header (name, state word, freshness, `#disconnect` visible), with the app frame left and the fact card right, side by side. Right (320px): "Needs you" list and the live activity log, plus `#simulate-change`. Agent facts is a tab beside the fact card (and its own table on wide screens).
**Mobile (390):** one column, no sideways scroll, 44px targets, bottom tab bar (Home, Connect, Memory, Settings). Reading order differs from desktop: Today sentence, then Needs you (Front Door first), then pebbles as a vertical list of full-width rows, with the selected service's summary card showing `#disconnect` on the first screen. Opening a service pushes a screen with Back and a three-segment control: Facts (first), App (frame, fallback link), Agent. `#add-service` is a primary button in Home and in the tab bar's Connect, and Connect is full-screen steps with a sticky Back/Next. `#simulate-change` and `#live-toggle` sit in Settings and a compact Live chip in the header. Confirmations use a bottom sheet. **Tablet (768, bonus):** icon rail replaces the bottom bar, the detail stacks app over facts.

