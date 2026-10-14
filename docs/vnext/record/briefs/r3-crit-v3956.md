# Worlds vNext real round: critique concept v3956's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v3956/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v3956

# v3956 · The service commonplace

**Design read:** A daily-use personal service shell for one person, with a calm, warm voice and a small expressive typographic signature. **Dials:** DESIGN_VARIANCE 5, MOTION_INTENSITY 3, VISUAL_DENSITY 4. This is a product interface, so the taste skill's landing-page blocks do not set its structure.

## 1. Metaphor in one line, and the main gesture.
Worlds is a commonplace book with labeled service entries: open an entry to see the owning app and the facts Worlds can share. Main gesture: select a service name, then move between its app and its shared facts.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start with Media Shelf (`healthy`) and Front Door (`needs_attention`); show Nightly Backup (`stale`) in the service register so three fixture services are present. Each entry names its owning app and keeps its own actions there. `#add-service` starts Notes Sync (`unavailable`) through Discover → Credential reference → Safe test → Preview → Choose what to publish → Publish. Accept a reference only, never a secret; show test failure and recovery without pretending a provider call occurred. Publish only checked fixture facts. A single shared in-memory fact store drives Home, the service view, and Agent facts. `#simulate-change` applies the fixture's `frontdoor.back-door` example to all three views together; it is a mock handoff/result, not a Worlds decision or real lock action.

## 3. Navigation between Worlds and a service (phone and desktop).
Persistent shell destinations are Home, Connect, Memory, Settings. Service entries sit within Connect; selecting one opens its detail while shell navigation remains available. On phone, selecting a service replaces the register with its detail and an explicit Back control. Keyboard order follows navigation, main, then complementary facts; focus is visible and all targets are at least 44px.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The selected service's own interface appears in a clearly bounded embedded frame beside its Worlds-owned fact summary on desktop, stacked after it on mobile. Keep service name, state in words, shell navigation, and a visible owner fallback around it. Use the fixture fallback links, including “Open Media app” (`app://media`) and “Open Home app” (`app://home`). If embedding fails, explain that the owning app still works and keep its link available. The mock frame receives no Worlds credential; actual browser isolation and app messaging remain unknown. Approvals hand off to Project Home; other actions hand off to the owning app.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Agent facts and human views use the same records, with each fact showing its source, observed/checked time, freshness, and separate Agent and Human permissions. Render `none` as a named disabled control with the fixture reason, never hide it. For `frontdoor.back-door`, the agent can read and the human can act; only the person decides whether to lock it. Routine freshness ticks update labels without announcing every poll. Announce meaningful state changes politely and sparingly.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
First run shows zero connected services and a clear Connect invitation; zero providers means zero readings. Quiet uses Media Shelf's exact `healthy` state. Needs-you preserves Front Door's `needs_attention` label and reason. Nightly Backup remains `stale`; unknown is said explicitly, never inferred as healthy. A failed test names what failed and offers retry or exit. Disconnect confirmation names the service and consequence; after removal, say what is gone while Home, Memory, and remaining entries continue working. Retention after disconnect is unknown. All states include readable text, not color alone. Show a visible MOCKUP label.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr's one integration feeding multiple surfaces, Dashy's in-place app opening, Home Assistant cards' shared state, and MCP ext-apps' contained UI boundary. This concept uses a quiet named-entry register and a paired app/facts reading surface rather than a widget wall or topology; each service's owner remains clear and authoritative.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `skill-taste-skill.md` and `ds-artistic.md`. Keep Artistic's expressive hierarchy and Limelight display face for short headings, but its high-saturation blue, pure-white broad surfaces, and high-motion defaults conflict with Worlds' comfort floor. Use its 4/8/12/16/24/32 spacing, 12/14/16/18/24/30/36 type scale, `#111827` text, and a small `#3B82F6` primary action accent only where contrast and low-glare checks pass; no secondary purple. Favor a soft neutral surface rather than large pure-white panels. Keep one radius system and one accent. Apply the skill's anti-default and copy self-audit guidance; use a self-contained HTML/CSS/vanilla JS prototype with no framework, CDN, external font, or image. Keep loading static, motion purposeful, and honor `prefers-reduced-motion`.

## 9. Signature element, hero thesis, reference lock.
Signature: a narrow, hand-drawn-looking inline SVG page corner that folds around the shared fact summary, marking “Worlds' copy” beside the owning app. Hero thesis: “Every service keeps its place. Every shared fact keeps its source.” Reference lock: Homarr's reactive integration reuse and Dashy's in-place app launch mechanics, not their visual language. Warmth comes from a small “You're all set” connected moment and kind recovery copy, not a character-driven theme.

## 10. Risks and UNKNOWNs.
The fixture is synthetic and every view must remain labeled MOCKUP, never a real reading. Connect protocol details, browser-enforced app isolation, cross-app messaging, and disconnect-history retention are unknown. The inline accent must not imply status or action. Verify text/focus contrast and reflow in the eventual prototype.

## 11. How it differs from C, and what it keeps
Unlike C's fixed cabinet bays and docked inset, this concept treats each service as a named entry in a commonplace register and pairs its app with a folded-corner shared fact note. It keeps C's real Home / Connect / Memory / Settings shell, explicit ownership, honest status words, persistent owner fallback, and the principle that Worlds composes facts while the owning app keeps its actions and authority.

## 12. Desktop and mobile layouts
At 1440px, a persistent left shell rail holds the four destinations; the main column shows the service register and selected service header, with the framed owning app and shared-facts note side by side. Keep `#add-service`, `#simulate-change`, `#live-toggle`, and a visible `#disconnect` for the selected service on the first screen. At 390px, use a dedicated bottom navigation for the four destinations; Home/Connect content becomes a single-column reading order, and a selected service gets its own detail view with Back, app frame, then facts. Keep the same four controls discoverable without horizontal scrolling, reserve safe-area space, and size all targets to at least 44px. A modest 768px layout may collapse the rail to a compact navigation row and stack app before facts. The live feed is on by default, advances fixture freshness/state in one shared store, and has a visible pause/resume control; state transitions are brief and gentle, with no nonessential motion under reduced-motion preference.

