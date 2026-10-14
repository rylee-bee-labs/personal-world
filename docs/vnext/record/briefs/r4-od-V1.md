# Build Worlds Concept 5 variation V1 from its DESIGN.md (a working prototype, MOCKUP)

Use the house design language: the Rylee-Bee Design System tokens (Mulberry & Mint, dark by default, mint accent #a7e6ce, teal focus ring #72b1b1, Atkinson Hyperlegible then system fonts, Young Serif then Georgia for display), soft contrast. Keep Dot the snail. Rylee: "the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive". Follow the DESIGN.md exactly; bind to the selected design system's tokens. It must work with real JS: (1) start with 2 connected services and add a 3rd live through discover -> credential reference (never a secret) -> safe test -> preview -> publish, via #add-service; (2) at least 3 fixture services, names and states verbatim; (3) an embedded app frame with a fallback 'open in the owning app' link (https://example.invalid/...), shell nav/back/status working around it; (4) #simulate-change updates home, the service surface and the agent-facts view at once, showing who sees and who acts; (5) a governed action hands off to the owning system; (6) #disconnect VISIBLE ON THE FIRST SCREEN for the selected service, confirm with a button labelled 'Disconnect', Worlds still works and says what is gone; (7) a live feed ON by default (freshness ticking, occasional stale) updating every view, with #live-toggle to pause/resume; optimistic feedback and visible calm transitions. Two purposeful layouts: 1440px desktop and 390px mobile (own nav pattern; no sideways scroll; 44px targets); keyboard reachable with visible focus; prefers-reduced-motion; stale/unknown in words; whimsy and warmth laced in (micro-copy, a small inline-SVG accent, gentle motion); a visible MOCKUP label. One self-contained HTML file: inline CSS and vanilla JS, no network, no external fonts/images/libraries. Exclude: emoji icons, lorem, invented metrics, hero gradients, indigo accent, hue-only status.

## DESIGN.md

# c5var V1: Counter-first (home-first) (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing is a real reading. A visible "MOCKUP" label sits in the top bar (desktop) and page header (mobile) on every screen, Settings and Connect included. This is Concept 5 ("The Counter", Dot the snail) in the house language (Rylee-Bee, Mulberry & Mint). Concept 5 §2 to §6, §9, §14 and §15 (integration model, six-step Connect, fact journey, honest fact rows, states, live feed, Dot) are kept unless named here. Concept 5 §12 and §13 are replaced by §1, §2 and §7 below.

**The V1 rule, and what makes the first screen different:** the counter is composed before you may connect, and the only route into a *service* is through its slip. Shell actions (`#add-service`, `#simulate-change`, `#live-toggle`, Memory, Settings) live in the top bar, rail and tab bar; they are never hidden inside a slip. The first-run counter has no slip, so its entry is the named `#add-service` under Dot's sleep line.
Testable on open: the first screen shows a **counter board** (a summary line above the slips: "2 services at the counter · 6 facts · 1 needs you", all computed from the store) and each slip carries its **headline fact** inline (Front Door "Back door: unlocked", Media Shelf "Items in library: 1,284"). Selecting a slip is how you reach its facts, actions, app frame and `#disconnect`. Nothing opens first: no wizard, no tour, no takeover.

## 1. Flow, step by step
Order of attention: composed home → embedded app → a change → disconnect, with Connect as a side action reached from home.

**Desktop, 1440:** top bar, 232px rail, 360px slip column, detail pane (flexible, min 640px).
1. Open on the counter. Top bar: MOCKUP, `#live-toggle` "Live · checked 24s ago", `#simulate-change`, `#add-service`, `#attention-count` "1 needs you". Rail: Home (`aria-current="page"`), Connect, Memory, Settings. Column: counter board line, then Front Door (preselected, "Selected", state word, reason) above Media Shelf. Within ten seconds: slips left, selected story right.
2. Pane: header (service name in Young Serif, state word, **`#disconnect` "Disconnect Front Door" on the first screen**), fact journey receipt (Source → Worlds → Agent / You), "Agent sees" strip, app frame strip with fallback "Open Home app".
3. Open a fact row: three stations expand in place. `frontdoor.guest-code-used` shows its named, disabled control with the fixture's reason verbatim.
4. `#simulate-change` applies `lock-back-door`; slip, headline fact, header, receipt, agent strip, board line and `#attention-count` re-settle in one render; "Mock handoff recorded; Worlds locked nothing." stays. With nothing else needing you: "That's the last one. Nothing needs you right now."
5. `#add-service` opens Connect (§2) as a side panel; Publish lands Notes Sync as a slip, selected, with Dot's parcel.
6. `#disconnect` opens a confirm dialog (Disconnect / Cancel): "{Name}'s {n} facts and its app frame are removed from Worlds; the app itself is untouched." Focus goes to the next slip. With none left: Dot asleep, the first-run line and `#add-service`.

**Mobile, 390:** own control map; exactly one bottom strip at a time (Concept 5 §3 table).
1. Page header: MOCKUP and title "Counter".
2. Status row: "1 needs you · checked 24s ago", then `#live-toggle` and `#simulate-change` as two 44px buttons (wrap to two lines if needed). Counter board line under it.
3. Selected-service card (Front Door): state word, reason, headline fact, "Open Front Door", **`#disconnect` on the first screen**.
4. Other slips, full width, 44px+ tall; floating `#add-service` (44px pill, bottom-right, `bottom: calc(3.5rem + env(safe-area-inset-bottom) + 1rem)`, `right: 1rem`); list bottom padding 8.5rem so it never covers a slip. Hidden on pushed screens and in Connect.
5. Tap a slip: pushed full screen (Back in top bar, tab bar hidden), same status row, `#disconnect` under the heading, in-page tabs App / Facts / Agent view. Focus moves to the new heading; Back returns focus to the invoking slip.
6. Connect: full-screen stepper (§2). Disconnect: bottom sheet over the tab bar with scrim. Tab bar (3.5rem + safe-area): Home, Connect, Memory, Settings.
**Tablet, ~768 (bonus):** rail collapses to icons (text on focus), slips become a two-column grid above the pane; Connect is a modal sheet.

## 2. Connect as a side action: the contract
The same six steps (Discover, Credential reference, Safe test, Preview, Choose, Publish) and the same buttons (`#step-back`, `#step-next`, `#run-test`, `#publish`) on both layouts, so they cannot drift.
- **Desktop:** a 480px panel docked right over the pane (`--surface-raised`, `--radius-lg`, 1px `--border-line`), `role="dialog" aria-modal="false"` labelled by its step heading. Not modal: the counter behind stays visible and live, and the top bar (`#live-toggle`, `#simulate-change`) stays operable; the rail and slips stay clickable (clicking a slip does not close the panel, and typed Connect state is kept). Layout inside: step list across the top ("Step 3 of 6, Safe test"), step body (scrolls), sticky footer with `#step-back` / `#step-next` (`#publish` replaces Next on step 6, `#run-test` sits in the step 3 body). Preview and Choose rows are one column: fact label, value, state word, source, then the checkbox, so a 480px panel never needs sideways scroll; long copy wraps.
- **Focus:** on open, focus goes to the step heading; Tab cycles through the panel then onward into the page (no trap, since it is non-modal). On each step change focus goes to the new step heading. **Escape closes** the panel and returns focus to `#add-service` (or to the rail Connect entry if that opened it). Closing mid-flow keeps the draft until Publish or an explicit "Discard".
- **Home stays current:** the rail keeps Home `aria-current="page"`; the rail Connect entry opens the same panel and is marked `aria-expanded`. The tick never pauses or blocks while the panel is open.
- **Done screen:** "Manage this reference in Settings" navigates to Settings › Credential references (panel closes). Settings' Back link, "Back to the counter", returns focus to the Notes Sync slip. On mobile this is the pushed Settings screen with Back in the top bar.
- **Mobile:** full-screen stepper, sticky Back/Next bar replaces the tab bar, same focus rules (heading on push, Back/Escape-equivalent returns to the floating `#add-service`).

## 3. What changed from Concept 5, and why
- **Skin:** Wise lime/black-on-off-white becomes the house language, because Rylee asked to bring Concept 5 into "our design language". Structure and copy are unchanged.
- **Home is the only start:** no first-run takeover or guided intro (that is Welcome-led). First run is a state of home.
- **Counter board and headline facts** are new on slips, so home reads as composed before anything is opened.
- **Connect is a side panel,** not a peer destination, so the counter keeps ticking while you connect; the rail entry keeps it reachable.
- **Slips are the entry for services** (open, disconnect, per-service state); shell actions stay in the bar and rail. Settings keeps per-service `#disconnect-{service-id}` rows.

## 4. House tokens and look
Copy `:root` and the dark block from `house/tokens.css`; dark is default, then the light, high-contrast and low-stimulation blocks. No colours, spacing or radii outside it.
- Surfaces: `--surface-base` page, `--surface-raised` slips, panes, Connect panel; `--surface-sunken` app frame and inset receipt. Text `--text-primary`, secondary `--text-soft`. Borders `--border-line`; `--border-line-strong` on the selected slip.
- Primary buttons (`#add-service`, Next, Publish): `--action-primary` with `--action-on-primary`.
- **Focus (decided):** one product-wide handle on every interactive element: `--focus` aliased to Worlds' teal `#72b1b1` (`worlds-mulberry-mint.css`) in dark and low-stimulation, and to `--focus-ring` in light and high-contrast (where the teal would fail). `outline: 0.1875rem solid var(--focus); outline-offset: 0.125rem`. Measured on dark: 6.58:1 on `--surface-base`, 5.75:1 on `--surface-raised`, 7.20:1 on `--surface-sunken` (≥3:1); the build rechecks the other themes.
- **State words** are presentation of the machine states (`LANGUAGE.md` allows short human labels that keep the meaning). The fixture's `stateLabel` is kept verbatim: **"Needs a human"** is the label for `needs_attention`, in `--status-needs_attention`; "Healthy" `--status-healthy`, "Stale" `--status-stale`, "Unknown" `--status-unknown`, "Unavailable" `--status-unavailable`, "Not configured" `--status-not_configured`. The fixture label wins over LANGUAGE.md's example "Needs you" because brief v2 requires fixture names and states verbatim; no new state is invented. Always a shape plus a label, never colour alone.
- Selected slip: raised surface + `--border-line-strong` at 0.125rem + the word "Selected" + a check shape.
- Spacing `--space-*`, radii `--radius-md` rows / `--radius-lg` cards, frame, panel / `--radius-full` pills, type `--type-*` (nothing under `--type-small`). Every length in rem. Target floor 44px (2.75rem), aliased from Play-Nice as `--target-min` (not a house token). Fonts: Atkinson Hyperlegible Next body, Young Serif display and service headings, system fallbacks, no external fonts.
- Warmth per `JOY-CURIOSITY-AND-CARE.md`: Dot and kind copy come after the machine truth, as invitations, never urgency. Motion: one curve `cubic-bezier(.2,0,0,1)`; 150ms state confirms (120ms mobile), 200ms dialogs and panel; reduced motion (system or Settings) means no transform, instant swaps, data still updates.

## 5. States
First run (Dot asleep, "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you.", `#add-service`; the board line reads "0 services at the counter"), quiet, needs-you, stale (Media Shelf simulated, "Nothing has changed since {time} that Worlds knows of." + "Check now"), unknown ("Worlds has no current reading for this and won't guess."), disconnected, loading ("Dot is fetching the facts…" + fallback link), empty preview ("This service offers no facts yet. Nothing is guessed."), long copy wraps. Start data: exactly `media-shelf` and `front-door`; Notes Sync only via Connect.
**Error, two kinds:**
- *Failed safe test* ("Simulate a failed test (mock)"): `lastError` "provider timed out", "last tried Ns ago", try N of 3. Copy: "That didn't go through, and the reference is still here. Try again whenever you like." At three: "That's three tries. Check the reference with the owning app, then edit it to start again."
- *A service drops off:* Settings › Live feed has "Simulate a service dropping off (mock)" (checkbox, off). On, Media Shelf's slip becomes Unavailable (labelled "simulated") and its pane says "Worlds can't reach Media Shelf right now. The last reading is from {time}." Recovery: "Check now" (restores Healthy) and the fallback "Open Media app"; the board line and `#attention-count` update. A connected Notes Sync already shows the same recovery shape ("provider timed out", attempts 2, "Check provider now" as a handoff to the Memory app, never an approval).

## 6. Live feed
Concept 5 §14 unchanged: mock clock anchored at `generatedAt` (first paint "checked 24s ago", ages always recomputed from `checkedAt`), tick 5s (Settings 10s / 30s), re-check Front Door 20s and Media Shelf 30s, one simulated Media Shelf stale event at 90s, never overwriting fixture states (only a Healthy, all-live service may age). `#live-toggle` (`aria-pressed`, on by default, "Live · checked 24s ago" / "Paused · frozen at 24s") pauses clock, ticks and events; `#simulate-change` works paused; `#settings-live` always agrees. One store keyed by fact id, one render pass updates slip, headline fact, board line, pane, receipt, agent strip, Memory, Settings and `#attention-count`; feed changes carry the word "simulated". The open Connect panel never pauses the tick.

## 7. The daily-use shell (1440 and 390)
- **Memory** (rail / tab bar): the aggregate Agent facts surface, same row component as the pane. Grouped by service in slip order; each row: label, value, freshness, source, checked time, **Agent can** and **Human can** as two lines, reason where one exists. 1440: single wide column beside the rail (rows two-up at 1440), service group headings jump-linked from a 200px sub-list. 390: one column, group headings collapsible (open by default), tab bar visible. A change from `#simulate-change` re-settles it live; a row's service name links back to its slip.
- **Settings** sits last in the rail/tab bar (it is a place you visit, not where you start). 1440: 200px section list + pane. 390: one scrolling column with section headings, tab bar visible. Sections in order: Connected services (per-service `#disconnect-{service-id}`, same confirm), Credential references (Replace, Remove, "Worlds stores a credential reference, never the secret."; placeholders `ref://{service-id}`; removing makes the service "Not configured" with "Reconnect" opening Connect step 2), Providers (Local Weather, "Not configured", `summary`, `emptyReason`, "Open provider settings"), Live feed (`#settings-live`, cadence, Reduce motion Follow system / Always, the drop-off simulator), About (MOCKUP, "Synthetic data only", UNKNOWNs, disabled "Keep history after disconnect": "Not decided yet; this mockup keeps nothing."). States: loading ("Dot is fetching your settings…"), empty (first-run line), error ("Couldn't save that. Nothing was changed. Try again.").
- Embedded frame, fact permissions and handoffs are Concept 5 §4 and §5 verbatim: fallbacks "Open Media app" / "Open Home app" / "Open Memory app"; failure "This app didn't load inside Worlds." / "Use the link below, or try again."

## 8. Differs from C, borrowed, OpenDesign
**Versus C ("Service cabinet"):** keeps the persistent Home / Connect / Memory / Settings shell, labelled owned services, the fact journey, honest states, and the verbatim fixture reasons and fallbacks. Differs in one testable interaction: C docks an app beside a static fact card and nothing further happens per fact; here a slip opens a pane whose fact rows are interactive three-station journeys that visibly re-settle on every change, and Connect happens beside a live counter instead of replacing it.
**Borrowed:** Homarr (one integration feeds UI and agent), Dashy (apps open in place), HA cards (literal state words), MCP ext-apps (contained UI under a host boundary). **Different:** a per-fact journey you open, not a topology or ledger, and a Connect that never takes the counter away.
**OpenDesign:** skill `frontend-design` (one direction, real states, self-review at both widths) with design system `ds-rylee-bee-studio` (the house tokens). Rules applied: state-coverage, accessibility-baseline, animation-discipline, anti-ai-slop (no emoji icons, gradients or invented metrics). Signature: the fact journey receipt; thesis "Every fact says where it came from and who can act on it."

## 9. Integration requirements and checker
(1) Connect adds Notes Sync live: discover → `ref://` reference (secrets refused) → test → preview → choose → publish; (2) three-plus fixture services composed; (3) owner frame with verbatim fallbacks, shell navigation intact; (4) one change updates Home, the service surface, Memory's Agent facts and Settings at once, Agent can / Human can as separate lines; (5) approvals hand off to Project Home, other actions to the owner app; (6) disconnect says plainly what is gone and Worlds keeps working.
Checker ids: `#add-service` (starts Connect: shell action, or under Dot when empty), `#disconnect` (first screen, selected service), `#simulate-change`, `#live-toggle`, MOCKUP label. Floors: 1440 and 390 designed on purpose, no sideways scroll, 44px targets, visible focus, reduced motion honoured.
UNKNOWN: embed isolation, Connect API, disconnect history, system/Atkinson/Young Serif font fidelity.

## Critique fixes
1. **Flow asserted, not designed:** the V1 rule is split explicitly (shell actions in bar/rail; slips are the entry for services; the empty counter's entry is `#add-service` under Dot). Added the counter board line and inline headline facts as the testable first-screen difference.
2. **Connect side action:** §2 pins panel width (480px), non-modal `role="dialog" aria-modal="false"`, step layout, focus in/out, Escape, Home stays current, feed controls operable, and the mobile stepper as the same steps and focus rules.
3. **House language:** focus ring committed (teal `--focus`, rem, contrast measured, flag dropped); "Needs a human" kept as the fixture label for `needs_attention`, with the reason it wins over LANGUAGE.md's "Needs you"; ring and border widths in rem.
4. **Daily shell:** §7 pins Memory and Settings at 1440 and 390, §5 adds the drop-off error and recovery, and §2 gives the Settings deep link with focus returning to the Notes Sync slip.
5. **Required sections:** §8 adds the difference from C, borrowed/different, and the OpenDesign starting point.


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
