# v8457 — The Counter (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing here is a real reading. A visible "MOCKUP" label sits in the top bar (desktop) and the page header (mobile) on every screen, Settings and Connect included.

## 1. Metaphor in one line, and the main gesture
Worlds is a friendly service counter: every service hands its facts across it, and the counter shows plainly where each came from, how fresh it is, who can see it and who can act. Main gesture: **pick a service slip, then open a fact to read its journey** (Source → Worlds → Agent / You). A small inline-SVG snail, **Dot**, is the counter's mascot: it sleeps when the counter is empty, nods when something changes, and carries the parcel when a service connects. Dot is not the data courier and does not move with the feed tick.

## 2. Integration model
Start: exactly `media-shelf` (Healthy) and `front-door` (Needs a human). `#add-service` opens Connect for `notes-sync` with six steps (Discover, Reference, Test, Preview, Choose, Publish). Real `<button>` ids: `#step-back`, `#step-next`, `#run-test`, `#publish`.
1. **Discover**: lists Notes Sync and its owner, Memory.
2. **Credential reference**: a reference only. A pasted secret (anything not shaped `ref://…`) is refused with "Worlds stores a credential reference, never the secret". Validates on blur; has pristine, dirty and submitted-pending states; Next stays disabled until valid.
3. **Safe test**: run / running / passed / failed. It passes first time by default; a labelled "Simulate a failed test (mock)" checkbox makes it fail with the fixture's `lastError` "provider timed out". A failure keeps the typed reference, shows "last tried Ns ago" and "try N of 3". After the third try the run button is disabled until the reference is edited.
4. **Preview**: `sync.provider-reachable` (Unavailable), `sync.local-pending` (3 notes, Live), `sync.last-synced` (Stale). Never merged.
5. **Choose**: one checkbox per fact, all ticked; at least one must stay ticked; text states what is shared.
6. **Publish**: Dot delivers a parcel, a "Connected" stamp lands, Notes Sync appears in the slips and in Settings. The done screen has "Manage this reference in Settings" (deep link to Settings › Credential references).

All in memory; no provider call. One store keyed by fact id feeds Home, the service surface, Memory's Agent facts view and Settings. `#simulate-change` applies `lock-back-door` (`crossViewExample`): `frontdoor.back-door` → locked, Front Door → Healthy, attention count 0, on all views at once (see §14). Worlds locks nothing; it records a mock handoff to Project Home.

## 3. Navigation between Worlds and a service
Four destinations everywhere: **Home** (slips, selected service, receipt), **Connect**, **Memory** (the aggregate Agent facts view: every connected service's fact rows, same row component), **Settings** (§13). Desktop: left rail. Mobile: bottom tab bar.
Tab-bar contract at 390px (exactly one bottom strip at a time):
| Screen | Bottom strip |
|---|---|
| Home, Memory, Settings | Tab bar (56px + safe-area) |
| Pushed service screen | None; Back is in the top bar |
| Connect stepper | Sticky Back/Next bar replaces the tab bar |
| Dialogs (disconnect) | Bottom sheet over the tab bar, scrim behind |
Focus moves to the new heading on every push and returns to the invoking slip on Back. Keyboard Escape closes dialogs and returns focus to the invoking button.

## 4. The embedded-app frame
A bordered frame (iframe/ext-app style, drawn) holds the owning app, headed by owner name and state word. It is a secondary strip under the receipt, not the hero. The fallback link is always at the frame edge, verbatim from the fixture: "Open Media app" (`app://media`), "Open Home app" (`app://home`), "Open Memory app" (`app://memory`). The frame gets no Worlds credential by intent; real browser enforcement and sandbox flags are UNKNOWN. If embedding fails the shell, facts and fallback stay, with the cause "This app didn't load inside Worlds." and recovery "Use the link below, or try again." Approvals hand off to Project Home; other actions to the owner. Retry on Notes Sync ("Check provider now", attempts 2, "provider timed out") is an owner handoff to the Memory app, never an approval.

## 5. The agent/human shared-facts model
Each fact row shows label, value, freshness word, source (local / external / none), checked time, **Agent can** and **Human can** as two separate labelled lines, plus the reason where one exists. `frontdoor.back-door`: Agent can read, Human can act ("Locking the door changes the external system and is the person's decision."). `frontdoor.unlocked-for`: both read. `frontdoor.guest-code-used`: Agent none, shown as a named, focusable, disabled control whose reason is the fixture's, verbatim ("Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who."). `sync.provider-reachable` stays Unavailable while `sync.local-pending` stays Live. Values and permissions never change except through `#simulate-change`.

## 6. States
- **First run (zero services):** Dot asleep beside an empty counter. Copy: "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you." and `#add-service`. Reached by disconnecting both services; Settings shows empty lists with the same line.
- **Quiet:** Media Shelf, Healthy. After `#simulate-change` with no other need: "That's the last one. Nothing needs you right now."
- **Needs-you:** Front Door, "Needs a human", with the fixture's `attentionReason`.
- **Stale:** see §14 for the simulated Media Shelf event; Notes Sync's last sync is Stale. Copy: "Nothing has changed since {time} that Worlds knows of." with a "Check now" button.
- **Unknown:** said in words, never blank: "Worlds has no current reading for this and won't guess."
- **Error:** failed safe test (§2) or a service that drops off. Copy: "That didn't go through, and the reference is still here. Try again whenever you like." At the limit: "That's three tries. Check the reference with the owning app, then edit it to start again."
- **Disconnected:** `#disconnect` is visible on the first screen for the selected service (§12) and opens a confirm dialog with a `Disconnect` button and a Cancel. Afterwards: "{Name}'s {n} facts and its app frame are removed from Worlds; the app itself is untouched." Focus goes to the next slip.
- **Loading:** static Dot and "Dot is fetching the facts…" with a recovery link to the fallback. **Empty preview:** "This service offers no facts yet. Nothing is guessed." **Long copy:** wraps, never truncates.

## 7. Borrowed, and what is different
Homarr: one integration feeds the UI and the agent. Dashy: apps open in place. HA cards: literal state words. MCP ext-apps: a contained UI under a host boundary. Different: the receipt is a per-fact journey that you open and that visibly re-settles when a change arrives. It is a list, not a map or topology, and unlike a static ledger of facts (round 2, anti-repeat) its rows are interactive stations, not a typographic read-out (see §11).

## 8. OpenDesign starting point
Skill `frontend-design` (commit to one direction; real states; self-review at both widths) and design system `ds-wise`.
**Committed look: Wise's confident black-type-on-off-white, rebuilt on system fonts.**
- Canvas `#f7f7f2` (off-white, small tint of the Wise warmth). Text `#0e0f0c`; secondary `#454745` (8.72:1 on canvas); `#868685` only for non-essential decoration (3.39:1, never text).
- Lime `#9fe870` with `#163300` text (9.45:1) on primary pill buttons only, never a large surface, never beside mint. Pills carry a 1px `#0e0f0c` ring because lime on canvas is 1.37:1.
- Selected slip: mint `#e2f6d5` fill **plus** a 2px `#0e0f0c` border (17.89:1 vs canvas), the word "Selected" and a check shape. Mint vs canvas is only 1.06:1 and lime vs mint 1.29:1, so neither colour is ever the indicator; the border, word and shape are. Text on mint: `#0e0f0c` 16.82:1, `#454745` 8.20:1.
- Needs-you: yellow `#ffd11a` pill with `#0e0f0c` text (13.17:1) and an alert-triangle shape. Error: `#d03238` icon/border with `#0e0f0c` text (the red itself 4.66:1 on canvas). Positive `#054d28` text (9.31:1). Each always with a label.
- Focus ring: 3px `#0e0f0c`, 2px offset (17.89:1 on canvas, 16.82:1 on mint, 13.05:1 on lime).
- Radii 16px (rows, inputs), 30px (cards, frame), 9999px (buttons, state pills). 8px spacing. Ring shadow `0 0 0 1px rgba(14,15,12,.12)`.
- **Type (no external fonts):** display `"Segoe UI Black","SF Pro Display","Roboto",system-ui,sans-serif`, weight 900, `font-feature-settings:"calt"` on all text. Service heading 40px/0.95 desktop, 28px/1.0 mobile; slip title 18/1.25 weight 600; row label and value 16/1.4 weight 600; sentences (reasons, copy) 16/1.5 weight 400; captions and state words 14/1.5 weight 600; nothing under 14px. Trade: Wise Sans 900 at 0.85 leading is abandoned because it cannot wrap safely in a calm product ("rows wrap, never truncate"); 0.95 keeps the heavy, stamped feel. Body 600 is used for labels and values (the confident voice), 400 for reading text, because system faces render 600 sentences too heavy.
- Scale-on-hover (1.03, not 1.05) on buttons only; off under reduced motion and on touch.
- Craft rules applied: state-coverage, accessibility-baseline, animation-discipline, anti-ai-slop (no emoji icons, gradients, invented metrics).

## 9. Signature, hero thesis, reference lock
ONE signature: the **fact journey receipt**. It is the structural spine of the detail pane; the app frame is a secondary strip beneath. Opening a fact row shows three stations in a row (wrapping to a column at 390px): **Source** (local / external / none, checked time), **Worlds** (fact id, freshness word), **Agent / You** (Agent can, Human can, reasons). When a change lands, the stations tick in order Source → Worlds → Agent/You (§15). Thesis: "Every fact says where it came from and who can act on it." Reference lock: Wise's transfer breakdown (the staged line items; mechanics only) and Apple Home's accessory states. C's docked app-beside-card pairing is **not** this concept's signature.

## 10. Risks and UNKNOWNs
UNKNOWN: embed support and isolation, the Connect API, whether disconnect retains history, and fidelity of system fonts across platforms. Whimsy risk is managed by §15 (Dot is a small accent, never in the way). Contrast of the signature pairs is measured above, before the build; the build must recheck only that the implemented hex values match.

## 11. How it differs from C, and what it keeps
Keeps C's persistent shell (Home / Connect / Memory / Settings), labelled and owned services, honest states, and the verbatim fixture reasons and fallbacks. Differs in one testable interaction: in C, selecting a bay docks an app beside a static fact card and nothing further happens per fact; in v8457, selecting a fact row opens its three-station journey (Source → Worlds → Agent / You), and every feed tick or `#simulate-change` visibly re-settles those stations in order, with the tick count, slip, pane and agent strip all updating in the same render. It differs from a "ledger of facts" because a ledger is a read-only typographic list; here each row is a disclosure with stations, a settle sequence and a per-fact permissions control. The skin is Wise (lime, heavy type), not C's skeuomorphic bezel; warmth comes from Dot and the copy lines in §15.

## 12. Desktop and mobile layouts
**Desktop, 1440px:** top bar (MOCKUP label, `#live-toggle`, `#simulate-change`, `#add-service`, attention count `#attention-count` "1 needs you"), 232px rail, 360px column of service slips (Front Door selected on load; each slip shows state word, freshness, and "Selected" when chosen), and a detail pane: service header (name, state word, **`#disconnect` "Disconnect Front Door" visible**), the fact journey receipt (primary), an "Agent sees" strip directly under it, then the app frame strip with its fallback link. `#simulate-change` visibly changes slip, header, receipt rows, agent strip and top-bar count together.
**Tablet, ~768px:** rail collapses to icons with tooltips and visible text on focus; slips become a two-column wrapped grid above the pane (no horizontal scroll).
**Mobile, 390px (own control map, not the desktop order):**
1. Page header: MOCKUP label and title.
2. **Counter card** (status row): one-line status ("1 needs you · checked 24s ago"), then `#live-toggle` and `#simulate-change` as two 44px-high buttons, wrapping onto two lines if needed.
3. **Selected service card** (Front Door first): state word, fixture reason, an "Open Front Door" button, and **`#disconnect` on the first screen**.
4. The other slips (full-width, state word + freshness, 44px+ tall).
5. Floating `#add-service` (44px pill, bottom-right): `bottom: calc(56px + env(safe-area-inset-bottom) + 16px)`, `right: 16px`; the list gets 136px bottom padding so it never covers a slip. Hidden on pushed screens and in Connect.
Opening a service pushes a full screen: Back (top bar), the same status row (`#live-toggle`, `#simulate-change`) and `#disconnect` under the heading, then in-page tabs **App / Facts / Agent view** (the frame sits in App with its fallback link beneath; Facts holds the receipt). Tab bar hidden (see §3). Connect is a full-screen stepper with a sticky Back/Next bar, the only bottom strip. No horizontal scroll; rows wrap, never truncate; every target is at least 44×44px with 8px gaps.

## 13. Settings (a real screen)
Sections, in order. Desktop: a left sub-list of sections (200px) and a pane; mobile: one scrolling column with section headings, no sub-list, tab bar visible.
1. **Connected services:** one row per connected service, with state word, freshness, and its own `#disconnect-{service-id}` (same confirm dialog). Empty: the first-run line from §6.
2. **Credential references:** list of references with "Replace" (inline field, same on-blur validation and secret refusal) and "Remove" (confirm). Heading note: "Worlds stores a credential reference, never the secret." Handles for Media Shelf and Front Door are UI placeholders in the form `ref://{service-id}` (not fixture data) and Notes Sync shows the one just entered. Removing a reference does not disconnect; the service becomes "Not configured", with a "Reconnect" button that opens Connect step 2 with the field empty. Empty: "No references stored. Connecting a service will add one."
3. **Providers:** Local Weather, state "Not configured", with the fixture's `summary` ("No weather provider has ever been set up. The tile holds zero data until the person chooses one; nothing is guessed."), `emptyReason`, and the fallback "Open provider settings" (`app://settings/providers`). No facts and no placeholder value.
4. **Live feed:** `#settings-live` switch (the same state as `#live-toggle`; the two always agree), cadence 5s (default) / 10s / 30s, "Reduce motion": Follow system (default) / Always. Explains that reduced motion removes animation, not data.
5. **About:** MOCKUP label, "Synthetic data only", and the §10 UNKNOWNs as a list. Disconnect history row: a disabled, named control "Keep history after disconnect" with reason "Not decided yet; this mockup keeps nothing."
States: loading (static Dot + "Dot is fetching your settings…"), empty (as above), error ("Couldn't save that. Nothing was changed. Try again."), long references wrap.

## 14. The live feed (model)
- **Clock:** a mock clock anchored at fixture `generatedAt` `2026-10-13T14:24:26Z`, advancing in real time. First paint reads real ages from `checkedAt`: Front Door "checked 24s ago", Media Shelf "28s ago". Ages are always recomputed from `checkedAt`, never taken from `ageSeconds`. No fabricated 2s.
- **Tick:** every 5s by default (Settings: 10s / 30s). Each tick recomputes every age and re-renders everything that shows one: slip, header, `#live-toggle` readout, receipt rows, agent strip, Memory, Settings.
- **Re-check:** live local services simulate a check on a schedule: Front Door every 20s, Media Shelf every 30s, setting `checkedAt` and `observedAt` of their live facts to the mock now. Values do not change. Stale, unavailable, unknown and not-configured services never re-check; their ages keep growing.
- **Staleness rule:** only a service that is Healthy and has only live facts may age. `needs_attention`, `unavailable`, `unknown`, `not_configured` and stale are never overwritten. Once per session, 90s after first load, Media Shelf misses a re-check: its state word becomes "Stale" (labelled "simulated"), summary becomes "Nothing has changed since {time} that Worlds knows of.", "Check now" restores Healthy. Front Door becomes eligible only after `#simulate-change` makes it Healthy.
- **Pause (`#live-toggle`, `aria-pressed`):** on by default, label "Live · checked 24s ago", paused label "Paused · frozen at 24s". Pausing freezes the mock clock, ticks, re-checks and the staleness event; data and actions stay usable; `#simulate-change` still works. Resume restarts the clock from where it stopped.
- **Propagation:** one store keyed by fact id, one render pass. A tick, a re-check, the staleness event and `#simulate-change` all re-render slip, detail pane, receipt, agent strip, Memory, Settings and `#attention-count` together. Every feed-driven change carries the word "simulated".
- **Optimistic actions:** `#simulate-change` shows its result instantly, the stations settle in order (§15), and a line "Mock handoff recorded; Worlds locked nothing." stays visible.

## 15. Warmth and motion
- **Dot:** idle pose is static; Dot nods once (200ms) when a state word changes, sleeps (closed eyes) at first run, and carries a parcel across on connect (400ms desktop, 300ms mobile). Dot appears at: first run, connect, empty states, and next to the "Updated" word on a change. The tick is not Dot's journey.
- **Copy:** the verbatim lines in §6 and §13 are the warmth on working screens, plus the Disconnect line and "Mock handoff recorded; Worlds locked nothing."
- **Motion spec:** one curve, `cubic-bezier(.2,0,0,1)`. State-change confirmation 150ms (120ms mobile): the state word cross-fades and a tick mark fades in at each station, staggered 60ms (Source, Worlds, Agent/You). Sheets and dialogs 200ms. Motion confirms a change that has already happened; nothing animates on a plain tick except the number.
- **Reduced motion** (system setting or Settings override): no transform or scale, no Dot travel or nod; changes swap instantly with opacity only, the stations show all ticked at once, and the word "Updated just now" stays for 3s. The connect stamp appears without travel. Data continues to update.

## Critique fixes
1. **Settings:** designed as §13 (five sections, desktop and 390 order, states), including credential references (list, replace, remove), Providers with the `local-weather` empty state verbatim, live-feed controls and About with UNKNOWNs. Connect deep-links to it.
2. **Live feed:** modelled in §14: mock clock at `generatedAt` (first paint "24s ago"), 5s tick, a staleness rule that never overwrites fixture states, pause semantics and explicit propagation to every view.
3. **Mobile:** own control map in §12 with `#live-toggle` and `#simulate-change` in the status row, a per-screen tab-bar table in §3, one bottom strip at a time, and safe-area placement for `#add-service`.
4. **Warmth:** pinned verbatim lines, one motion spec with a reduced-motion substitute, and Dot's role redefined (reacts, does not carry the tick) in §15; "slow on purpose" dropped.
5. **Direction:** system type stack and sizes committed (§8); one hero (the receipt, §9); §11 rewritten as one testable interaction difference and separated from "ledger of facts"; selected-slip contrast measured now.
