# Worlds vNext real round: critique concept V1's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept5-variations-brief.md`, `docs/vnext/concept5/DESIGN.md`, `docs/vnext/house/` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/c5var/V1/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, faithfulness to Concept 5, a clearly distinct overall flow, correct use of the house design language (tokens, Mulberry & Mint, LANGUAGE.md words). Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for V1

# c5var V1: Counter-first (home-first) (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing is a real reading. A visible "MOCKUP" label sits in the top bar (desktop) and page header (mobile) on every screen. This is Concept 5 ("The Counter", Dot the snail) dressed in the house language (Rylee-Bee, Mulberry & Mint), with the flow organised around one rule: **the app opens on the composed counter, everything starts from a slip, and Connect is a side action.** Concept 5 §2-§6, §9, §13, §14 and §15 (integration model, six-step Connect, fact journey, honest fact rows, states, Settings, live feed, Dot) are kept unless named below.

## 1. Flow, step by step
Empty → connect → composed home → embedded app → a change → disconnect. Here the order of *attention* is home first; the empty and connect steps are reached from home, not before it.

**Desktop, 1440:**
1. Open: the counter. Top bar (MOCKUP, `#live-toggle` "Live · checked 24s ago", `#simulate-change`, `#add-service`, `#attention-count` "1 needs you"), left rail (Home, Connect, Memory, Settings), a slip column with Front Door pre-selected ("Needs a human", with `attentionReason`) above Media Shelf, and the detail pane. Within ten seconds: slips on the left, the selected one's story on the right.
2. Pane for the selected slip: header with state word and **`#disconnect` ("Disconnect Front Door") visible on the first screen**, then the fact journey receipt (Source → Worlds → Agent / You), the "Agent sees" strip, then the app frame strip with its fallback ("Open Home app").
3. Open a fact row: its three stations expand in place. `frontdoor.guest-code-used` shows the named, disabled control with the fixture's reason.
4. Change: `#simulate-change` applies `lock-back-door`; slip, header, receipt, agent strip and `#attention-count` all re-settle in one render; "Mock handoff recorded; Worlds locked nothing." stays. If nothing else needs you: "That's the last one. Nothing needs you right now."
5. Connect (side action): `#add-service` opens Connect as a **side panel over the pane** (not a route away), six steps with `#step-back`, `#step-next`, `#run-test`, `#publish`. The counter stays visible and live behind it; on Publish Dot carries the parcel and a Notes Sync slip lands in the column, selected.
6. Disconnect: `#disconnect` → confirm dialog (Disconnect / Cancel) → "{Name}'s {n} facts and its app frame are removed from Worlds; the app itself is untouched." Focus goes to the next slip. Disconnect both and the counter shows Dot asleep with the first-run line and `#add-service`.

**Mobile, 390** (own control map; one bottom strip at a time, per Concept 5 §3):
1. Page header (MOCKUP, title), then the status row: "1 needs you · checked 24s ago", `#live-toggle` and `#simulate-change` as two 44px buttons.
2. Selected-service card (Front Door): state word, reason, "Open Front Door", **`#disconnect` on the first screen**.
3. The other slips, full width. Floating `#add-service` bottom-right above the tab bar (safe-area aware), list padded so it never covers a slip.
4. Tap a slip → pushed full screen (Back in top bar, tab bar hidden), same status row and `#disconnect` under the heading, in-page tabs App / Facts / Agent view.
5. Connect: a full-screen stepper with a sticky Back/Next bar replacing the tab bar. Disconnect: bottom sheet with scrim. Tab bar: Home, Connect, Memory, Settings.

## 2. What changed from Concept 5, and why
- **Skin:** Wise lime/black-on-off-white is replaced by the house language, because Rylee asked to bring Concept 5 into "our design language". Structure and copy are unchanged.
- **Home is the only start:** there is no first-run takeover or guided intro (that is the Welcome-led variation). First run is a state of home, not a mode.
- **Connect becomes a side action** (side panel on desktop, pushed stepper on mobile) so a person can see the counter keep ticking while they connect. In Concept 5 it was a peer destination; the rail entry stays so it is still reachable.
- **Slips are the entry for everything:** open, disconnect, change and Settings deep links all start from a slip; Settings' per-service `#disconnect-{service-id}` rows remain.

## 3. House tokens
Copy `:root` and the dark block from `house/tokens.css`; dark is default, light/high-contrast/low-stimulation blocks follow it. No colours, spacing or radii outside it.
- Surfaces: `--surface-base` page, `--surface-raised` slips and panes, `--surface-sunken` the app frame and inset receipt. Text `--text-primary`, secondary `--text-soft` (soft contrast, never pure white). Borders `--border-line`; `--border-line-strong` for the selected slip.
- Primary buttons (`#add-service`, Next, Publish): `--action-primary` fill with `--action-on-primary` text. Focus: 3px ring, `--focus-ring` token, with Worlds' teal `#72b1b1` from `worlds-mulberry-mint.css` as the front-door ring if the build keeps that contract; recheck ≥3:1 on all three surfaces.
- State words always beside a shape and a label, never colour alone: Needs a human `--status-needs_attention`, Healthy `--status-healthy`, Stale `--status-stale`, Unknown `--status-unknown`, Unavailable `--status-unavailable`, Not configured `--status-not_configured`. Human labels follow `LANGUAGE.md` (Needs you = `needs_attention`); no new state is invented.
- Selected slip: raised surface + 2px `--border-line-strong` + the word "Selected" + a check shape.
- Spacing `--space-*`, radii `--radius-md` (rows) / `--radius-lg` (cards, frame) / `--radius-full` (pills), type `--type-*` (nothing under `--type-small`). Lengths in rem. Targets 44px, aliased from Play-Nice as `--target-min` (not a house token). Fonts: Atkinson Hyperlegible Next (body) and Young Serif (display, service headings) first, system fallbacks, no external fonts.
- Warmth per `JOY-CURIOSITY-AND-CARE.md`: Dot and kind copy appear as invitations after the machine truth, never creating urgency. Motion: one curve `cubic-bezier(.2,0,0,1)`, 150ms state confirms, 200ms dialogs; reduced motion (system or Settings) means no transform, instant swaps, data still updates.

## 4. States
First run (Dot asleep, "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you."), quiet, needs-you, stale (Media Shelf simulated, "Check now"), unknown ("Worlds has no current reading for this and won't guess."), error (failed safe test via "Simulate a failed test (mock)": `lastError` "provider timed out", try N of 3), disconnected, loading ("Dot is fetching the facts…" plus fallback link), empty preview, long copy wraps. Wording is Concept 5 §6 verbatim. Start data: exactly `media-shelf` and `front-door`; Notes Sync is added only by Connect.

## 5. Live feed
Concept 5 §14 unchanged: mock clock anchored at `generatedAt` (first paint "24s ago", ages recomputed from `checkedAt`), 5s tick (Settings 10s/30s), re-check Front Door 20s and Media Shelf 30s, one simulated Media Shelf stale event at 90s, never overwriting fixture states. `#live-toggle` (`aria-pressed`, on by default) pauses clock, ticks and events; `#simulate-change` still works paused; `#settings-live` always agrees with it. One store keyed by fact id, one render pass updates slip, pane, receipt, agent strip, Memory, Settings and `#attention-count`; feed changes carry the word "simulated". Home-first adds one rule: the open Connect panel never pauses or blocks the tick.

## 6. Integration requirements met
(1) Connect adds Notes Sync live through discover → reference (`ref://` only, secrets refused) → test → preview → choose → publish; (2) three-plus fixture services composed; (3) owner app frame with fallback links verbatim, shell navigation intact; (4) one change updates Home, service surface, Memory's Agent facts and Settings at once, with Agent can / Human can as separate lines; (5) approvals hand off to Project Home, other actions to the owner app; (6) disconnect says plainly what is gone and Worlds keeps working.
Checker ids: `#add-service`, `#disconnect` (first screen, selected service), `#simulate-change`, `#live-toggle`, plus MOCKUP label. Floors: 1440 and 390 designed on purpose (768 bonus), no sideways scroll, 44px targets, keyboard focus visible, focus moves to the new heading on push and back to the invoking slip.

UNKNOWN: embed isolation, Connect API, disconnect history, font fidelity; the focus ring choice (house vs Worlds teal) is flagged for Rylee.

