# Worlds vNext real round: critique concept v4629's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v4629/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v4629

# v4629 — Windowsill

MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing here is a real reading. Single self-contained HTML later; this file pins decisions first.

## 1. Metaphor in one line, and the main gesture.
Worlds is a sunny windowsill of paper-wrapped seed packets: one labelled packet per service, each owned by its app, each with a tiny drawn sprout whose posture says how it is doing (the words say it first). The main gesture is **pick a packet, and its app opens in a clipped frame with the fact slip tucked beside it**. Warmth is in the sprouts and the copy; the structure stays a calm app.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start: `media-shelf` (Media Shelf, `healthy`) and `front-door` (Front Door, `needs_attention`, "Needs a human"). `#add-service` starts Connect for `notes-sync` in five real `<button>` steps: Discover → Credential reference (reference only; a pasted secret is refused with "Worlds keeps a reference, never the secret") → Safe test (idle/running/failed/passed; first run shows the fixture error "provider timed out" with "Try again") → Preview (`sync.provider-reachable` `unavailable`, `sync.local-pending` 3 notes `live`, `sync.last-synced` `stale`) → Publish (a checkbox per fact). Success is the "connected" moment: the new packet's seed sprouts (one 400ms transform, static under reduced motion) and the line "Notes Sync is on your sill." One in-memory store keyed by fact id feeds Home, the packet, the fact slip and Agent facts. No provider is called.
`#simulate-change` applies `lock-back-door` to `frontdoor.back-door` (`crossViewExample`): fact → `locked`, Front Door → `healthy`, `attentionCount` → 0, in one update across Home, the Front Door packet/slip and Agent facts, with a polite live-region announcement. It is a mock handoff recorded for review; Worlds locks nothing. `#disconnect` is on the first screen for the selected service (Front Door at first load), opens a confirm naming it, and its confirming button reads "Disconnect".

## 3. Navigation between Worlds and a service (phone and desktop).
Persistent destinations: Home, Connect, Memory, Settings (Agent facts is a tab of Home and of each service). Desktop: left rail. Phone: bottom tab bar, and a selected service opens as a full-height sheet with explicit Back; focus goes to the sheet heading and returns to the invoking packet. Source order: nav, main, facts.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The owning app is drawn as a framed window "clipped" to the packet, titled with its owner. Fallback sits at the frame edge, verbatim from the fixture: Media Shelf "Open Media app" (`app://media`), Front Door "Open Home app" (`app://home`), Notes Sync "Open Memory app" (`app://memory`); links are `app://` only. The frame is meant to carry no Worlds credential; browser-enforced sandbox flags and messaging are UNKNOWN. If it cannot load, the packet, slip, status and Back stay, and the failure and owner link are shown. Approvals hand off to Project Home; other actions to the owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Fact slip and Agent facts read the same records. Columns: source, checked time (live-recomputed), freshness word, Agent can see, Human can see, Who can act. `frontdoor.back-door`: agent `read`, human `act` ("Locking the door changes the external system and is the person's decision."). `frontdoor.guest-code-used`: agent `none` is a named, focusable, disabled control with its exact reason ("Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who."). `frontdoor.unlocked-for`: both `read`. Provider failure and local data stay separate records (`sync.provider-reachable` `unavailable` beside `sync.local-pending` `live`).

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
- First run (zero services): an empty sill with one bare pot, "Nothing planted yet", and an Add service button; no invented readings.
- Quiet: Media Shelf, "Healthy". Needs-you: Front Door, "Needs a human" plus its fixture reason.
- Stale (live feed): every tick ages "checked 2s ago" → "12s ago"; once a service passes 45s unchecked it moves to "Stale" in words, the sprout droops, and "Check now" resets it. Unknown: never blank; reads "Unknown" with the reason. Error: safe-test failure, embed failure, and Notes Sync `unavailable` each give cause and a next step, and keep input.
- Disconnected: the packet is removed, the sill closes up, and a line says "Front Door is no longer connected; its facts are gone from Home and Agent facts." Everything else keeps working. Retention after disconnect is UNKNOWN.
- Feed: ON by default; `#live-toggle` shows "Live: on / paused" and a "Paused" word beside the clock. Reduced motion stops animation, not data. Actions are optimistic ("Locking… → Locked"), then settle visibly.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Homarr: one integration feeds the UI and the assistant. Dashy: app opens in place. HA cards: literal state words. MCP ext-apps: contained UI under a host boundary. Different: no map, routes or topology; independent owners in a flat list, joined only by the shared fact store.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Skill `soft-skill` (high-end visual design) and `ds-paper`. Kept from the skill: the double bezel (outer hairline tray, inner core, concentric radii) for packets and the frame; pill buttons; custom cubic-bezier, transform/opacity-only motion; macro-whitespace; collapse to one column under 768px with `100dvh`. Rejected, because this is a daily app: scroll-blur entrances, glass blur on content, the floating-island nav, huge section padding, dark/glow archetypes. Fonts: Roboto/Montserrat (ds-paper) and Inter/Helvetica (skill) both conflict with the no-external-fonts rule, so system serif for headings (`ui-serif, Georgia`) and `system-ui` for UI. From `ds-paper`: tokens `#111111` primary/CTA, `#111827` text, `#FFFFFF` surface, 4/8/12/16/24/32 spacing, 14/16/18/24/32/40 type; success `#16A34A`, warning `#D97706`, danger `#DC2626` are only for small marks beside a state word (contrast checked on built pairs; failures fall back to `#111827`). Purple `#8B5CF6` is dropped (anti-slop indigo list); at most two accent uses per screen. Paper feel comes from hairlines and radii, no grain. Also applied: state-coverage (five states per surface), animation-discipline (≥150ms, motion only for change of state), accessibility-baseline, no emoji icons, no lorem, no invented metrics.

## 9. Signature element, hero thesis, reference lock.
Signature: the seed packet with its drawn inline-SVG sprout (upright, drooping, flagged, bare soil, sprouting on connect), always paired with a state word. Hero thesis: "Every service gets a packet, and every packet tells the truth." Reference lock: Apple Home's accessory-to-room status handoff (mechanics) and a Muji-style labelled stationery layout (labels and hairlines only).

## 10. Risks and UNKNOWNs.
Embed support and isolation, the Connect API and its real lifecycle, and disconnect history are UNKNOWN. The sprout cute factor could outrun the words, so words lead and the sprout never carries status alone. Live ticks could be noisy for screen readers, so only state changes are announced, politely. White-on-white relies on hairline contrast of 3:1, to be measured. Fixture is synthetic.

## 11. How it differs from C, and what it keeps
Keeps: the persistent Home / Connect / Memory / Settings shell, the app docked beside the shared fact card, owner and fallback always visible, the six-step Connect story for Notes Sync, the exact permission rows, and honest states. Changes: C's static cabinet bezel becomes a warm packet-and-sprout object with a drawn character per state; C is explicitly motionless, while this one is live (tick, stale drift, optimistic settle, connect sprout) and calm under reduced motion; the selected packet and Disconnect live on the first screen, not behind a register; copy is kind and specific ("Nothing planted yet"); the fact slip clips to the frame as a paper note rather than a fixed card; and the mobile layout is its own design.

## 12. Desktop and mobile layouts
**Desktop (1440px):** three columns. Left rail (Home, Connect, Memory, Settings, the MOCKUP label, and `#live-toggle` with the live clock). Centre: the sill, a grid of packets (Home summary on top: "1 needs you" and the quiet count, `#add-service` as a bare-pot packet). Right, the selected-service dock: header with name, state word and `#disconnect`, then the framed owning app with the owner fallback link, then the fact slip, then `#simulate-change`. Agent facts is a tab beside Home. Hover shows a lift, focus a 3px ring.
**Mobile (390px):** one column, own reading order: status line and clock with `#live-toggle`, "needs you" first, then the selected service card with its sprout, state word and `#disconnect`, then the list of other packets, then `#add-service` as a full-width button. Bottom tab bar (four 44px+ targets). Tapping a packet opens a sheet: heading, state, fact slip (facts before the app because it reads faster), framed app, owner link, Back. `#simulate-change` sits in a "Try it" row under the selected card. Connect steps are full-screen, one at a time, sticky Continue above the tab bar. No sideways scroll. Tablet (~768px, bonus): rail collapses to icons; the dock becomes a sheet.

Checker ids: `#add-service`, `#disconnect` (first screen, confirm button "Disconnect"), `#simulate-change`, `#live-toggle` (ON by default).

