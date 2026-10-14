# Worlds vNext real round: critique concept v3604's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v3.md`, `docs/vnext/concept-brief-v2.md`, `docs/vnext/reference/direction-C-DESIGN.md` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/round3/v3604/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, difference from C and the anti-repeat list, real use of its OpenDesign skill/design system. Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for v3604

# v3604 — The service shelf

## 1. Metaphor in one line, and the main gesture.
A calm service shelf gives each independent app a labeled place, while a shared “what Worlds knows” slip follows the selected service. The main gesture is **choose a service, then compare its own screen with the facts Worlds shares**. A small hand-drawn paper tab marked “from this app” adds warmth without turning the interface into a storybook.

## 2. Integration model: how a service plugs in, where it lives in the shell, how its facts flow.
Start with `media-shelf` (Media Shelf, `healthy`) and `front-door` (Front Door, `needs_attention`); their owner links and fixture facts remain literal. `#add-service` opens a six-step Connect flow for `notes-sync` (Notes Sync, `unavailable`): discover; enter a credential reference (never a secret); safe test; preview `sync.provider-reachable`, `sync.local-pending` and `sync.last-synced`; choose facts to publish; publish. Each service owns its data and actions. Worlds composes selected facts into Home and Agent facts from one in-memory fixture store. `#simulate-change` applies the fixture’s `lock-back-door` example to `frontdoor.back-door`, updating Home, the Front Door surface and Agent facts together; it is a mock handoff/review, never a real lock action. `#disconnect` confirms the service by name and removes its shelf entry while other Worlds destinations remain usable.

## 3. Navigation between Worlds and a service (phone and desktop).
Desktop keeps Home, Connect, Memory and Settings in a persistent left rail. A compact service index sits beside the selected service’s framed app, with the Worlds-owned fact slip adjacent and visibly separate. On mobile, a persistent bottom bar carries those four destinations; the service index becomes a list screen and a selected service gets a detail screen with an explicit Back button. The reading order remains navigation, main content, then complementary facts. No critical action moves into hover or overflow.

## 4. The embedded-app frame (sandbox, fallback, what the shell keeps around it).
The owner app sits in a distinct, restrained frame labeled with service name, state and “owned by” app. The host navigation, current service state and shared fact slip stay outside the frame. Show fixture fallback links (`Open Media app`, `Open Home app`, `Open Memory app`) at the frame edge, including when the embed is unavailable. A missing or failed frame leaves its explanation and owner link in place. The mock frame receives no Worlds credential; browser isolation, sandbox flags and app messaging remain UNKNOWN. Decisions hand off to Project Home for governed Home operations, and to the owner app for other service actions.

## 5. The agent/human shared-facts model (source, freshness, who sees, who acts).
Home, the service detail and Agent facts read the same fact IDs. Every fact row states its source, checked/observed time, freshness in words, who can see it, and who can act. Preserve the fixture distinction on `frontdoor.back-door`: Agent can read, Human can act, because locking changes the external system. Keep `frontdoor.guest-code-used` human-readable; show Agent “none” and its fixture reason without exposing `visitor-3`. On the live Front Door screen, `#disconnect` is visible on the first screen. A gentle live tick updates “checked … ago” everywhere, may mark one service stale, and settles optimistic action feedback visibly. `#live-toggle` is on by default and pauses/resumes that feed; data continues under reduced motion, while decorative motion stops.

## 6. States: empty/first run (zero services, zero providers), quiet, needs-you, stale, unknown, disconnected.
Zero services offers a small drawn empty shelf, says no services are connected, and points to Connect; zero providers means no readings are invented. Quiet uses Media Shelf’s exact `healthy` state. Needs-you preserves Front Door’s `needs_attention` label and reason. Stale names what is old and when it was checked; unknown says no current observation, as for Garden Camera. Unavailable remains distinct from stale: Notes Sync’s provider can be unavailable while three local notes remain live. Disconnected explains which service left. Connect also has loading, validation, failed safe-test, empty preview and long-copy states, each with cause and recovery; no spinner without a timeout. Status always has words as well as restrained semantic color.

## 7. Borrowed from Homarr / Dashy / HA cards / MCP ext-apps, and what is deliberately different.
Borrow Homarr’s one integration reused across surfaces, Dashy’s in-place app access, Home Assistant cards’ shared state, and MCP ext-apps’ contained UI boundary. The shelf makes source and ownership explicit at the moment facts are compared; it does not turn services into a map, cable patchbay or floating window collection. Worlds frames and composes; the service remains its owner.

## 8. Your OpenDesign starting point (skill + design system) and how you used it; which guide rules you applied.
Starting point: `skill-minimalist-skill.md` and `ds-storytelling.md`. Use the minimalist skill’s flat editorial hierarchy and restrained warm monochrome as structure; use Storytelling’s playful voice and one hand-drawn paper tab as the small narrative accent. Keep its spacing scale (4/8/12/16/24/32), text token `#111827`, and use the named palette tokens only where they remain readable; reject its Inter/large-white-surface defaults where they conflict with the minimalist rulebook and low-glare contract. Apply anti-slop rules (specific fixture copy, no gradients, emoji or invented metrics), color/typography contrast and hierarchy, state coverage, labeled native controls, and Laws of UX grouping/proximity. Use the craft rulebooks’ 44px targets, visible focus, semantic order, static default feedback, and reduced-motion behavior. Any final color pair must meet WCAG AA before implementation.

## 9. Signature element, hero thesis, reference lock.
Signature: the “from this app” paper tab visually bridges a framed owner app to its shared fact slip, and repeats on Home and Agent facts. Hero thesis: **“Every service keeps its own screen; every shared fact keeps its source.”** Reference lock: Homarr 2.0’s reusable integration mechanic and Apple Home’s accessory-to-home state relationship; borrow the relationship, not their styling.

## 10. Risks and UNKNOWNs.
Real Connect protocol, provider sandbox enforcement, app messaging, disconnect history retention and exact live freshness policy are UNKNOWN. The fixture is synthetic; label the prototype MOCKUP and never imply a real reading. The paper illustration must not carry status meaning. Verify text/component contrast, 390px reflow without horizontal scroll, 44px targets, keyboard/focus behavior and reduced-motion treatment in the built artifact.

## 11. How it differs from C, and what it keeps
C’s fixed cabinet bays and docked app-plus-fact pairing are the starting point. v3604 shifts the visual thesis from a cabinet of places to a **service shelf with a source tab**: the small “from this app” marker is the single signature, and the shelf/list is intentionally light rather than a physical cabinet. It keeps the real Worlds shell, clear service ownership, honest state labels, owner fallback, shared facts and an embedded app framed by persistent Worlds navigation. It does not reuse C’s inset-bezel cabinet treatment.

## 12. Desktop and mobile layouts
At 1440px, use a persistent left navigation rail, a narrow service index, and a generous selected-service region split between the owner frame and adjacent fact slip; keep the first-screen service state and `#disconnect` visible without opening another surface. Home and Agent facts reuse the same fact-row grammar. At 390px, use the bottom navigation, then a single-column service index/detail flow; the detail places service name/state, `#disconnect`, owner frame and facts in that order, with Connect and `#simulate-change` reachable as labeled full-width actions. Keep `#live-toggle` visible near freshness. The Connect steps become one-column screens with explicit Back/Continue buttons. All targets are at least 44px, safe areas are respected, and content reflows at 200% zoom with no sideways page scroll.

