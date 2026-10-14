# Worlds vNext real round: critique concept v4220's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v4220/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v4220

# v4220 — Service folio

**MOCKUP direction; no prototype or reading is approved.** Worlds is the frame around independent services, not their replacement.

## 1. Metaphor in one line, and the main gesture.
Worlds is a service folio: each service has a clearly named page, while its shared facts travel as small, source-stamped slips. The main gesture is to open a service page, see its own app beside the facts Worlds shares, and choose a clear next step.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start with Media Shelf (`healthy`) and Front Door (`needs_attention`); present Nightly Backup (`stale`) as the third composed service, using fixture IDs, names, states, and facts verbatim. `#add-service` begins Notes Sync (`unavailable`) in six named stages: discover; enter a credential reference only (never a secret); run a safe test; inspect preview; choose facts to publish; publish. Preview keeps `sync.provider-reachable` (`unavailable`), `sync.local-pending` (3 notes, `live`), and `sync.last-synced` (`stale`) distinct. Each service owns its app and actions; Worlds composes selected facts in Home and Agent facts. `#simulate-change` applies the fixture's `lock-back-door` example to `frontdoor.back-door`, updates Front Door to `healthy` and Home attention count to 0, and refreshes the Agent facts row in the same store update. It is mock review/handoff only; Worlds does not lock the door.

## 3. Navigation between Worlds and a service (phone and desktop).
Persistent Home / Connect / Memory / Settings navigation frames every view. Desktop keeps the navigation rail visible; a readable service index opens a two-column service page with the owning app and shared fact slip side by side. Mobile uses a bottom navigation bar and single-column pages: service index, then selected service detail, with a visible Back control. The first Connect screen shows `#disconnect` for the selected service without opening another panel. Preserve semantic source order, keyboard access, visible focus, and 44px targets.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The service page contains a bounded, labeled app frame with owner name and a visible fixture fallback: “Open Media app” (`app://media`), “Open Home app” (`app://home`), or “Open Memory app” (`app://memory`). Worlds navigation, back, service state, and shared facts remain outside and around the frame. If embedding fails, explain that state and keep the owner link and facts available. The mock frame receives no Worlds credential; actual browser isolation, sandbox flags, and app messaging are UNKNOWN. Approval handoff goes to Project Home; other actions go to their owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
The same fact record feeds Home, the service page, and Agent facts. Every row gives fact label/value, source, observed or checked time, explicit freshness/state, and separate agent/human visibility and action permissions. Render `none` as a named disabled control with its fixture reason, never hide it. For `frontdoor.back-door`, the agent reads and the human acts: “Locking the door changes the external system and is the person's decision.” For `frontdoor.guest-code-used`, the agent's disabled permission explains: “Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who.” Facts retain their independent states; a failed provider does not erase locally available data.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
Zero services offers Connect and a useful local Worlds/Memory baseline; zero providers shows no invented readings. Quiet is Media Shelf's `healthy`. Needs-you uses Front Door's exact state and fixture reason. Nightly Backup stays `stale`; unknown is stated in words with its reason, never inferred as healthy. Disconnect names the service and requires confirmation with a button labeled “Disconnect”; after removal, say plainly what is gone while other destinations remain usable. Include a failed service-test state with recovery and an embedding-unavailable fallback. `#live-toggle` starts ON and pauses/resumes fixture freshness ticks; stale transitions update every view together. Motion is restrained, one-shot, and removed under `prefers-reduced-motion`; data updates continue. No status relies on color alone.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's shared integration feeding several views, Dashy's named service checks, Home Assistant's explicit stale/unknown states, and MCP ext-apps' contained UI boundary. As the research note flags these mechanics as memory-based rather than live-verified, treat them as pattern references, not claims about current products. This folio keeps the service's owning app visible beside its shared facts and puts person-readable provenance ahead of provider-shaped controls.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `skill-minimalist-skill.md` + `ds-pinterest.md`. Carry forward warm sand neutrals, plum-black text, generous but purposeful page spacing, and rounded folio sheets; use no Pinterest brand red, external imagery, or invented font asset. Type uses an intentional system sans stack with fallback, clear hierarchy, and 15–18px body copy. A single hand-drawn inline SVG bookmark/tab marks the current service; it is decorative and hidden from assistive technology. Apply the craft guides: one signature move, named tokens and restrained accent, no gradients/emoji/filler/invented metrics, explicit state coverage, 4.5:1 body and 3:1 large/control contrast, keyboard-visible focus, reduced-motion support, and calm optimistic feedback. Label every view MOCKUP.

## 9. Signature element, hero thesis, reference lock.
Signature: a small “fact slip” that appears unchanged on Home, beside its owner app, and in Agent facts; its source and freshness remain attached wherever it appears. Hero thesis: “A service keeps its own page. A fact keeps its source.” Reference lock: Notion's document-like hierarchy and Apple Home's service-to-state relationship; borrow mechanics only, not their palettes or skins.

## 10. Risks and UNKNOWNs.
Connect protocol details, actual credential-reference handling, browser-enforced embedding isolation, service messaging, and disconnect history are UNKNOWN. The live feed is a synthetic mock, not a real reading. The stationery metaphor must not imply that facts are manually authored or detached from their owners. Contrast needs implementation checking.

## 11. How it differs from C, and what it keeps
C's fixed service cabinet pairs each owner bay with a docked app and fact card. The folio keeps C's real Home / Connect / Memory / Settings shell, independent service ownership, honest states, owner fallback, and shared human/agent facts, but makes the portable fact record—not a cabinet bay—the signature. Service pages read as warm editorial documents, with the same fact slip reused across views; there are no cabinet compartments, physical bezel, or simulated hardware.

## 12. Desktop and mobile layouts
At 1440px, keep the four destinations in a persistent left rail, the service index in a narrow middle column, and the selected service's embedded app beside a fact column in the main canvas. The full first-screen Connect surface shows the selected service and `#disconnect`; keep `#add-service`, `#simulate-change`, and `#live-toggle` plainly visible. At 390px, use a persistent four-item bottom bar and a single-column reading order: service heading/state, owner app frame and fallback, shared facts, then actions. The index and detail are separate screens with an explicit Back button; no sideways scrolling. Keep all controls at least 44px, allow text reflow at 200% zoom, and preserve the same state and actions without relying on desktop hover or motion.

