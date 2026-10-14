# Build Worlds Concept 5 variation V2 from its DESIGN.md (a working prototype, MOCKUP)

Use the house design language: the Rylee-Bee Design System tokens (Mulberry & Mint, dark by default, mint accent #a7e6ce, teal focus ring #72b1b1, Atkinson Hyperlegible then system fonts, Young Serif then Georgia for display), soft contrast. Keep Dot the snail. Rylee: "the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive". Follow the DESIGN.md exactly; bind to the selected design system's tokens. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.

## DESIGN.md

# V2 — Welcome-led Counter (MOCKUP)

## 1. Metaphor and main gesture
Worlds is a small welcome at a service counter: Dot the snail helps place the first two service slips, then rests beside the counter’s freshness line while the facts do the work. First-run gesture: connect a service and learn how one fact travels. Daily gesture: choose a slip, open a fact, and follow its receipt from Source → Worlds → Agent / Human. Dot is decorative inline SVG, never telemetry or a gate.

## 2. Integration model
The welcome-led variation starts at zero connected services and zero providers; this changes only requirement 1’s initial state. The settled counter begins with the fixture’s `media-shelf` and `front-door`, then `#add-service` remains available to add the third fixture service, `notes-sync`, through Discover → credential reference → safe test → Preview → Choose → Publish. This makes requirements 1 and 2 demonstrable from the settled counter; Discover names Media Shelf and Front Door for first run and Notes Sync thereafter. Names, states, facts, reasons, and owner routes come only from `fixture.json`; no invented service data. The three `sync.*` facts appear in Preview with their fixture values and states.

Credentials accept references only, never secret values. The prototype stores UI-only handles `ref://media-shelf` and `ref://front-door` for the welcome services, explicitly not fixture data; Notes Sync stores the entered reference in memory. Each service and action remains owned by its named app. Worlds composes selected facts into Home, Memory’s Agent facts view, Settings, and the service surface. Connect is a six-step flow; test progress is static, first test passes by default, and an explicit mock failure uses fixture `lastError` where present. Preview lets the person choose facts; Publish immediately adds the service slip and a “Connected” receipt. No provider calls occur.

`#add-service`, `#disconnect`, `#simulate-change`, and `#live-toggle` are real, labelled controls on the first screen. With zero services `#disconnect` is visible and disabled with “Connect a service before disconnecting”; once selected, it is enabled. Confirmation names the service and says its Worlds facts and app frame will be removed while the owning app remains untouched.

## 3. Flow and navigation
**Desktop (1440px):** persistent top bar carries MOCKUP, live controls, `#add-service`, and selected service’s `#disconnect`; a left rail keeps Home, Connect, Memory, Settings reachable. First-run Home opens on a broad welcome card: sleeping Dot, brief explanation, honest “First service” progress, and “Connect a service.” Connect proceeds through Discover, Reference, Test, Preview, Choose and Publish, then a concise receipt offers “Add one more” or “See your counter.” After the second publish, Home reveals the normal counter. “Skip welcome” goes directly to the empty counter; after first run Home opens directly on the counter. Service selection shows the full-width slip list beside a detail pane ordered slip → receipt → embedded-app frame; the receipt is the hero and the frame is secondary. Selecting a fact opens its three-station receipt.

**Mobile (390px):** first-run reading order is page header with MOCKUP/title, status line and `#live-toggle` / `#simulate-change`, selected-service area (or honest empty line), Dot and welcome copy, then `#add-service`. The empty line says no services are connected yet. Connect is a full-screen stepper with named step and progress, one decision per view, and a sticky Back/Next bar replacing the tab bar. The receipt offers “Add one more” and “See your counter.” On the settled counter: header; one-line status (“1 needs you · checked 24s ago”) with the two 44px controls; selected-service card with state, fixture reason, owner-open action and `#disconnect`; remaining full-width slips; floating `#add-service` at `right: 16px; bottom: calc(56px + env(safe-area-inset-bottom) + 16px)`, with list padding so it cannot cover a slip. Tapping a slip pushes a service screen: Back in the top bar, tab bar hidden, `#disconnect` under the heading, and in-page **Facts / App / Agent view** tabs. Facts holds the receipt; App holds the framed app and owner fallback. The receipt comes first inside the pushed service screen.

**At ~768px:** rail collapses to labelled icons (labels appear on focus), slips wrap into a two-column grid above the detail pane; all controls remain reachable with no horizontal scroll. At every width, browser zoom and text scaling remain available; at 200% zoom content reflows without clipping or sideways page scroll. Mobile Home is a selectable slip list that pushes a focused service screen, not a long desktop-detail stack. On Home, Memory, and Settings the bottom tab bar is 56px plus safe area; pushed service screens have no tab bar; Connect’s step bar replaces it; confirmation is a modal sheet. Only one bottom strip appears at a time.

## 4. Embedded-app frame
Opening a service keeps Worlds navigation, state, MOCKUP label and back path visible around a labelled frame for the owning app. Facts and receipt remain primary; the frame is secondary. Use each fixture owner’s app name, `appRoute`, and fallback verbatim: for example “Open Home app” → `app://home`, “Open Media app” → `app://media`, “Open Memory app” → `app://memory`; Local Weather’s fallback is “Open provider settings” → `app://settings/providers`. If embedding is unavailable or disabled, preserve facts, state and fallback and explain “This app didn’t load inside Worlds.” Offer retry/open-owner choices. Real sandbox policy and supported embedding mechanism are UNKNOWN; never grant the embedded app Worlds credentials. Consequential actions hand off to the owning app. For Front Door this is Home at `app://home`; Worlds does not lock the door.

## 5. Shared facts and live feed
One in-memory store keyed by fixture fact id feeds counter slips, service details and Memory’s Agent facts view. Rows show value (or an explicit Unknown/Unavailable label), state, source, checked/observed time, recomputed freshness, and separate “Agent can” / “Human can” permissions with reasons. “You can” is not used: fixture permission `human` is presented as “Human can”; “Needs you” maps only to `needs_attention`; each service’s `stateLabel` is shown verbatim. Permission `none` remains a named disabled item with its fixture reason.

The mock clock starts at fixture `generatedAt` (`2026-10-13T14:24:26Z`); first paint is Front Door “checked 24s ago” and Media Shelf “checked 28s ago.” Recompute age from `checkedAt`, never `ageSeconds`. Tick every 5s by default (Settings: 10s / 30s); each tick recomputes ages and updates slips, header, receipt, Memory, Settings and attention count in one render pass, without reload. Simulated re-checks update `checkedAt`/`observedAt` for live Front Door every 20s and Media Shelf every 30s; values do not change. Never re-check `needs_attention`, `unavailable`, `unknown`, `not_configured`, or already stale services; their age grows and their state words remain.

Once per session, 90s after first load, Media Shelf misses a re-check only while Healthy with all facts live: mark it “Stale (simulated)” and show “Nothing has changed since {time} that Worlds knows of.” with “Check now”; Check now restores Healthy. `#live-toggle` has `aria-pressed`, is on by default, and reads “Live · checked 24s ago” or “Paused · frozen at 24s.” Pause freezes clock, ticks, re-checks and staleness event; actions and `#simulate-change` remain usable. Resume continues from the frozen time. Routine ticks are not announced; batch meaningful live announcements politely.

`#simulate-change` applies fixture `crossViewExample` atomically: `frontdoor.back-door` becomes “locked”, Front Door becomes Healthy, attention count moves 1 → 0 in Home, service detail and Memory. Keep “Mock handoff recorded; Worlds locked nothing.” visible and offer the fixture owner route to Home. Publish and disconnect apply their visible results optimistically in the same frame (new slip appears; removed slip, facts and frame disappear), then settle visibly; afterwards name what was removed. Motion confirms changes already applied.

## 6. States
- **Empty / first run:** zero services/providers; Dot sleeps beside an empty counter; invite connection without fabricated progress. Local Weather remains Not configured, with no facts or placeholder value.
- **Quiet:** “Nothing needs you right now”; Healthy and current are explicit words, without celebratory noise.
- **Needs you:** service state label “Needs a human” and fixture decision reason; action remains with Home.
- **Stale:** preserve last known value and checked time; say Stale, offer Check now/owner route without promising recovery.
- **Unknown:** “Worlds has no current reading for this and won’t guess,” with the fixture’s reason when present; null is never blank or healthy.
- **Disconnected:** confirm service name and consequence; afterwards report the removed Worlds facts/frame and that the owning app remains.
- **Working / failed:** Connect test has static progress, specific failure, preserved reference, retry and recovery guidance. Status always has a text label; color and Dot expression are supplemental.
- **Loading / empty preview:** static Dot and “Dot is fetching the facts…” with fallback; “This service offers no facts yet. Nothing is guessed.”

## 7. Borrowed mechanics, distinct flow
Borrow Homarr’s shared integration for interface and agent facts, Dashy’s in-place owner app, Home Assistant cards’ shared state subscriptions, and MCP ext-apps’ contained app boundary. Deliberately different: first run is a warm, optional invitation that earns its welcome through two service slips, then settles into the counter; service UI and permissions do not become Worlds authority. The receipt keeps the semantic journey visible rather than adopting a provider-shaped dashboard.

## 8. Settings (a real screen)
Desktop uses a section sub-list beside the settings pane; mobile is one scrolling column with section headings. Sections appear in this order:

1. **Connected services:** one row per connected fixture service with its exact state label, freshness, and its own `#disconnect-{service-id}` action using the same confirmation. Empty copy: “Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you.”
2. **Credential references:** list each stored reference with Replace and Remove. Heading note: “Worlds stores a credential reference, never the secret.” Media Shelf and Front Door show `ref://{service-id}` UI handles, not fixture data; Notes Sync shows the reference entered in this mock session. Remove confirms, then makes that service Not configured and offers Reconnect into Connect step 2 with an empty field; it does not disconnect the service. Empty copy: “No references stored. Connecting a service will add one.”
3. **Providers:** Local Weather, state “Not configured”; fixture summary verbatim: “No weather provider has ever been set up. The tile holds zero data until the person chooses one; nothing is guessed.” Show `emptyReason` verbatim: “No provider configured, so there are no facts to show. The concept must render the empty state honestly rather than a placeholder value.” No facts or placeholder value. Fallback “Open provider settings” → `app://settings/providers`.
4. **Live feed:** `#settings-live` switch is the same state as `#live-toggle` and they always agree; cadence choices 5s (default), 10s, 30s; “Reduce motion” choices Follow system (default) / Always. Explain: reduced motion removes animation, not data.
5. **About:** MOCKUP, “Synthetic data only,” and this design’s UNKNOWNs. Include disabled named “Keep history after disconnect” with reason “Not decided yet; this mockup keeps nothing.”

Settings states: static Dot and “Dot is fetching your settings…” while loading; empty states as above; save error “Couldn’t save that. Nothing was changed. Try again.” References wrap without truncation. Connect’s publish receipt links to Settings › Credential references.

## 9. OpenDesign and house language
Start from OpenDesign `frontend-design` and the assigned `user:rylee-bee-studio` / Rylee-Bee Design System, Mulberry & Mint. Use canonical `--surface-*`, `--text-*`, `--border-*`, `--action-*`, `--focus-ring`, `--status-*`, `--accent-*`, `--space-*`, `--radius-*`, and `--type-*` token names and values. Dark is default; light, high-contrast and low-stimulation token modes remain supported. Use Atkinson Hyperlegible Next for body and Young Serif for display, with system fallbacks and no external fonts. Focus uses one shared source: `--focus-ring` from tokens.css in every theme; do not override it with the legacy teal value from the older Worlds application stylesheet. Mint is the dark-mode action colour; in light, high-contrast and low-stimulation modes use `--action-primary` as defined by tokens.

Use the token type ladder directly: body `--type-body`, labels `--type-label`, lead copy `--type-lead`, and welcome/display h1 at `--type-h1` in Young Serif; compose larger welcome emphasis only from `--type-h1` plus spacing and weight, never an un-tokened size. Apply state coverage for empty/loading/failed/unknown, color and typography discipline, visible keyboard focus, 44px targets, static loading, motion/reduced-motion rules, skip link and semantic landmarks. No gradients, emoji icons, filler or invented metrics. Warmth is Dot’s quiet parcel and precise, kind copy, not forced celebration.

## 10. Signature, hero thesis, reference lock
**Onboarding signature:** Dot carrying the first service slip. **Daily signature:** the fact journey receipt, whose Worlds station includes “Connected during first run · reference held, never the secret” for welcome-connected services; later connections use the same provenance row. In a change, stations settle Source → Worlds → Agent/Human with 60ms staggering. On desktop the receipt is primary above the app frame; on mobile it is first in Facts on the pushed service screen. Thesis: “A first connection teaches how Worlds keeps facts and ownership clear; every later fact keeps that promise visible.” Reference lock: Homarr 2.0 reactive integration pairing and Apple Home’s legible accessory state language (mechanics only, no borrowed palette).

Compared with Concept 5, V2 moves the first-run invitation and two connections ahead of the composed counter; afterward the daily interaction stays distinct through the welcome provenance carried in each receipt, while the receipt remains the daily hero. Compared with C, it keeps the persistent shell, named owner services, shared factual state, and in-place owner app, while replacing the initial service cabinet with a skippable welcome and making the daily fact journey the primary detail surface.

## 11. Warmth, motion, and unknowns
Dot sleeps at first run; rests beside the counter freshness line on working screens; appears once beside “Updated” when a meaningful state changes, never on a plain tick. One motion curve: `cubic-bezier(.2,0,0,1)`. State-word confirmation is 150ms (120ms mobile), with receipt stations staggered 60ms; dialogs/sheets 200ms; Dot’s parcel 400ms desktop / 300ms mobile. No motion on routine feed ticks except the age number. Reduced motion removes travel, transform and scale; state changes use opacity only, receipt stations appear ticked together, the connect stamp appears without travel, and “Updated just now” remains for 3s. Data continues. Respect OS reduced motion unconditionally. Working-copy warmth includes the stale line above and, on a service drop-off: “That service stopped answering. Its last facts are still here, marked with when Worlds checked.”

UNKNOWN: production Connect API and credential-reference semantics; embedded-app sandbox/browser support; disconnect-history policy; live-feed transport/cadence; whether first run should suggest exactly two services or simply offer two. Prototype uses only synthetic fixture data and makes no provider calls. Verify contrast, focus, reflow, screen-reader order and checker behavior in the built prototype at desktop and phone sizes; this design is not runtime or accessibility verification.

## Critique fixes
1. **Settings and third service:** adds a complete Settings destination and Local Weather provider fallback; Connect can discover Notes Sync with its three `sync.*` preview facts, so the settled counter supports adding a third fixture service.
2. **Daily signature:** keeps the receipt as the daily hero and carries welcome connection provenance in its Worlds station; specifies the desktop/mobile placement and station settle order.
3. **Reactive model:** sets initial ages, 5s default tick, 10s/30s choices, 20s/30s re-checks, deterministic stale event, pause semantics, attention propagation and optimistic publish/disconnect feedback.
4. **Mobile flow:** specifies status controls on settled Home, pushed service details with in-page tabs, tab-bar replacement rules, 768px behavior and safe-area placement.
5. **Warmth and motion:** places Dot on working screens, adds kind service-drop copy, one motion spec and its reduced-motion substitute.
6. **Corrections:** pins focus to the canonical token, clarifies action color across modes, maps Human to fixture `human`, names the fixture owner Home, and composes heading scale only from canonical type roles.


## fixture.json (the only data)

```json
{
  "$schema": "worlds-vnext/fixture/1",
  "_meta": {
    "purpose": "One constant, synthetic data fixture shared by every concept in the 30-concept Worlds exploration. Same services, states, facts and permissions for all concepts, so concept briefs differ only in interaction, never in data.",
    "synthetic": true,
    "syntheticNote": "Every name, host and value here is invented for the exploration. Links use the app:// scheme only; any placeholder host added later must use the reserved .example.invalid domain. There are no real secrets, hosts, tokens, accounts or personal data. If a real service is ever needed, it is added as a new field, never by editing these values in place.",
    "fixtureVersion": "1",
    "generatedAt": "2026-10-13T14:24:26Z",
    "statesVocabulary": {
      "source": "docs/accessibility/ACCESSIBILITY_CONTRACT.md §1.3 (canonical)",
      "service": [
        "healthy",
        "needs_attention",
        "unavailable",
        "stale",
        "unknown",
        "disabled",
        "not_configured"
      ],
      "fact": [
        "live",
        "stale",
        "unavailable",
        "unknown"
      ],
      "rule": "Status is never communicated by colour or icon alone; every state has an explicit text label (ACCESSIBILITY_CONTRACT §1.3–1.4). A blank value is never a state — an absent fact is rendered as unknown, unavailable or not_configured."
    },
    "permissionVocabulary": {
      "agent": ["read", "act", "none"],
      "human": ["read", "write", "act", "none"],
      "rule": "A fact carries an agent permission and a human permission independently. 'read' may see the value; 'act' may change the external system through the owning app; 'write' may edit the local copy only; 'none' must render as a named, disabled control with its reason, never hidden (accessibility floor; design-research pattern 10)."
    },
    "provenanceFields": {
      "checkedAt": "ISO-8601 UTC instant this observation was last checked.",
      "source": "Who produced the observation. 'local' = the local app answered; 'external' = a remote provider answered; 'none' = no source could be reached.",
      "ageSeconds": "Seconds between checkedAt and the fixture's generatedAt. A viewer should recompute age from checkedAt rather than trusting this number."
    }
  },
  "ownerFallback": {
    "rule": "Every service names the owning app and a fallback link. The service tile is a view over another system's data; the owning app keeps authority, and the fallback is the plain view shown when an embedded tool is unavailable or disabled (design-research patterns 5, 7, 13).",
    "kindNote": "'app' = another in-product app; 'external' = a provider's own page."
  },
  "services": [
    {
      "id": "media-shelf",
      "name": "Media Shelf",
      "kind": "media library",
      "state": "healthy",
      "stateLabel": "Healthy",
      "summary": "Library reachable and indexing is current.",
      "owner": {
        "app": "Media",
        "appRoute": "app://media",
        "fallback": {
          "label": "Open Media app",
          "href": "app://media",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T14:23:58Z",
        "ageSeconds": 28,
        "source": "local",
        "state": "live"
      },
      "facts": [
        {
          "id": "media.items",
          "label": "Items in library",
          "value": 1284,
          "unit": "items",
          "state": "live",
          "observedAt": "2026-10-13T14:23:58Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "media.indexing",
          "label": "Indexing backlog",
          "value": 0,
          "unit": "items",
          "state": "live",
          "observedAt": "2026-10-13T14:23:58Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "media.played-recently",
          "label": "Played in the last 7 days",
          "value": 12,
          "unit": "items",
          "state": "live",
          "observedAt": "2026-10-13T14:23:58Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        }
      ]
    },
    {
      "id": "front-door",
      "name": "Front Door",
      "kind": "home access",
      "state": "needs_attention",
      "stateLabel": "Needs a human",
      "summary": "Back door has been unlocked for longer than the person's usual window.",
      "attentionReason": "A decision is required; the local app will not act on its own.",
      "owner": {
        "app": "Home",
        "appRoute": "app://home",
        "fallback": {
          "label": "Open Home app",
          "href": "app://home",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T14:24:02Z",
        "ageSeconds": 24,
        "source": "local",
        "state": "live"
      },
      "facts": [
        {
          "id": "frontdoor.back-door",
          "label": "Back door",
          "value": "unlocked",
          "unit": null,
          "state": "live",
          "observedAt": "2026-10-13T14:24:02Z",
          "source": "local",
          "permissions": {
            "agent": "read",
            "human": "act",
            "humanReason": "Locking the door changes the external system and is the person's decision."
          }
        },
        {
          "id": "frontdoor.unlocked-for",
          "label": "Unlocked for",
          "value": 47,
          "unit": "minutes",
          "state": "live",
          "observedAt": "2026-10-13T14:24:02Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "frontdoor.guest-code-used",
          "label": "Guest code last used by",
          "value": "visitor-3",
          "unit": null,
          "state": "live",
          "observedAt": "2026-10-13T14:20:11Z",
          "source": "local",
          "permissions": {
            "agent": "none",
            "agentReason": "Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who.",
            "human": "read"
          }
        }
      ]
    },
    {
      "id": "nightly-backup",
      "name": "Nightly Backup",
      "kind": "backup",
      "state": "stale",
      "stateLabel": "Stale",
      "summary": "Last successful run is older than the configured daily window.",
      "owner": {
        "app": "Settings",
        "appRoute": "app://settings/backups",
        "fallback": {
          "label": "Open Backup settings",
          "href": "app://settings/backups",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T06:00:00Z",
        "ageSeconds": 30266,
        "source": "local",
        "state": "stale"
      },
      "facts": [
        {
          "id": "backup.last-success",
          "label": "Last successful backup",
          "value": "2026-10-09T06:00:00Z",
          "unit": null,
          "state": "stale",
          "observedAt": "2026-10-09T06:00:00Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "backup.last-attempt",
          "label": "Last attempt",
          "value": "failed: destination unreachable",
          "unit": null,
          "state": "stale",
          "observedAt": "2026-10-13T06:00:00Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "backup.size",
          "label": "Last backup size",
          "value": 4.2,
          "unit": "GB",
          "state": "stale",
          "observedAt": "2026-10-09T06:00:00Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        }
      ]
    },
    {
      "id": "garden-camera",
      "name": "Garden Camera",
      "kind": "camera",
      "state": "unknown",
      "stateLabel": "Unknown",
      "summary": "Disconnected since a restart; the local app has no current observation and will not invent one.",
      "owner": {
        "app": "Home",
        "appRoute": "app://home/cameras",
        "fallback": {
          "label": "Open Home cameras",
          "href": "app://home/cameras",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T11:02:00Z",
        "ageSeconds": 12146,
        "source": "none",
        "state": "unknown"
      },
      "unknownReason": "The device stopped answering at the fixture's baseline; no source has answered since.",
      "facts": [
        {
          "id": "camera.last-frame",
          "label": "Last frame",
          "value": null,
          "unit": null,
          "state": "unknown",
          "observedAt": null,
          "source": "none",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "camera.reachability",
          "label": "Reachable",
          "value": null,
          "unit": null,
          "state": "unknown",
          "observedAt": null,
          "source": "none",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "camera.human-detected-today",
          "label": "Human detected today",
          "value": null,
          "unit": null,
          "state": "unknown",
          "observedAt": null,
          "source": "none",
          "permissions": {
            "agent": "none",
            "agentReason": "The agent may not assert a detection while the source is unknown.",
            "human": "read"
          }
        }
      ]
    },
    {
      "id": "studio-printer",
      "name": "Studio Printer",
      "kind": "printer",
      "state": "unavailable",
      "stateLabel": "Unavailable",
      "summary": "The last job failed; the printer is reachable but rejected the job. Retry is offered.",
      "owner": {
        "app": "Studio",
        "appRoute": "app://studio/printers",
        "fallback": {
          "label": "Open Studio printers",
          "href": "app://studio/printers",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T14:22:40Z",
        "ageSeconds": 106,
        "source": "local",
        "state": "live"
      },
      "retry": {
        "label": "Retry last job",
        "action": "retry-last-job",
        "allowed": "human",
        "attempts": 1,
        "lastError": "job rejected: no media loaded"
      },
      "facts": [
        {
          "id": "printer.status",
          "label": "Printer status",
          "value": "error",
          "unit": null,
          "state": "unavailable",
          "observedAt": "2026-10-13T14:22:40Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "printer.queue",
          "label": "Jobs queued",
          "value": 1,
          "unit": "jobs",
          "state": "live",
          "observedAt": "2026-10-13T14:22:40Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "printer.last-error-code",
          "label": "Last error code",
          "value": "E-MEDIA-02",
          "unit": null,
          "state": "live",
          "observedAt": "2026-10-13T14:22:40Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        }
      ]
    },
    {
      "id": "notes-sync",
      "name": "Notes Sync",
      "kind": "sync",
      "state": "unavailable",
      "stateLabel": "Unavailable",
      "summary": "The sync provider did not answer its last two checks. Local notes still work; retry is offered.",
      "owner": {
        "app": "Memory",
        "appRoute": "app://memory",
        "fallback": {
          "label": "Open Memory app",
          "href": "app://memory",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": "2026-10-13T14:20:00Z",
        "ageSeconds": 266,
        "source": "none",
        "state": "stale"
      },
      "retry": {
        "label": "Check provider now",
        "action": "probe-provider",
        "allowed": "human",
        "attempts": 2,
        "lastError": "provider timed out"
      },
      "facts": [
        {
          "id": "sync.provider-reachable",
          "label": "Provider reachable",
          "value": null,
          "unit": null,
          "state": "unavailable",
          "observedAt": "2026-10-13T14:20:00Z",
          "source": "external",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "sync.local-pending",
          "label": "Local notes waiting to upload",
          "value": 3,
          "unit": "notes",
          "state": "live",
          "observedAt": "2026-10-13T14:23:30Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        },
        {
          "id": "sync.last-synced",
          "label": "Last successful sync",
          "value": "2026-10-13T13:58:00Z",
          "unit": null,
          "state": "stale",
          "observedAt": "2026-10-13T13:58:00Z",
          "source": "local",
          "permissions": { "agent": "read", "human": "read" }
        }
      ]
    },
    {
      "id": "local-weather",
      "name": "Local Weather",
      "kind": "weather",
      "state": "not_configured",
      "stateLabel": "Not configured",
      "summary": "No weather provider has ever been set up. The tile holds zero data until the person chooses one; nothing is guessed.",
      "owner": {
        "app": "Settings",
        "appRoute": "app://settings/providers",
        "fallback": {
          "label": "Open provider settings",
          "href": "app://settings/providers",
          "kind": "app"
        }
      },
      "freshness": {
        "checkedAt": null,
        "ageSeconds": null,
        "source": "none",
        "state": "unknown"
      },
      "facts": [],
      "emptyReason": "No provider configured, so there are no facts to show. The concept must render the empty state honestly rather than a placeholder value."
    }
  ],
  "crossViewExample": {
    "_comment": "A worked, synthetic example of design-research pattern 1 (shared fact, many views): one action on one fact updates several views. Concepts must not mutate the fixture; they may use this as the dependency to dramatise.",
    "factId": "frontdoor.back-door",
    "action": "lock-back-door",
    "observedBy": [
      "home tile (Front Door service state)",
      "attention list on the Bridge",
      "world health summary count"
    ],
    "resultingState": {
      "frontdoor.back-door": "locked",
      "front-door.state": "healthy",
      "attentionCount": 0
    }
  }
}

```
