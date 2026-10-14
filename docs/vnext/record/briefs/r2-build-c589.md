# Worlds vNext round 2, step 3: fix your DESIGN.md, then build concept c589

## Decisions in force
- 2026-10-13 Rylee on round 1: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page."
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept-brief-v2.md` (integration requirements 1-6, floors, output contract), `docs/vnext/fixture/fixture.json` (the only data), and the craft rulebooks in `docs/vnext/od-pack/`. Your OpenDesign starting point for this concept (skill, design system): frontend-design hud — their files are in docs/vnext/od-pack/.

1. Write `docs/vnext/round2/c589/DESIGN.md`: your design below, revised to fix every gap in the critique below (add a short "Critique fixes" section saying how each gap was fixed, or why not).
2. Then build `docs/vnext/round2/c589/index.html` FROM that DESIGN.md: one self-contained file (inline CSS and vanilla JS, no network, no fonts or images from outside). It must actually demonstrate, with working JS: (1) adding a 3rd service live through discover → credential reference → test → preview → publish, starting from 2 connected; (2) at least 3 fixture services composed; (3) an embedded app frame with a fallback link, the shell's nav/back/status still working around it; (4) one change updating home, the service surface and the agent-facts view at once, with who-sees/who-acts; (5) a governed action handed off to the owning system; (6) disconnect with Worlds still working. Give the key controls these ids so a checker can find them: `#add-service`, `#disconnect` (on any connected service), and `#simulate-change`. Phone 390px with no sideways scroll, keyboard reachable, reduced motion, stale/unknown in words, visible MOCKUP label.
Write only those two files. Do not push.

---
## Your DESIGN.md (to revise)

# c589 — The Instrument Desk

Status: DESIGN.md, written before any code. Nothing here is built yet.
Brief: [concept-brief-v2.md](../../concept-brief-v2.md) · Data:
[fixture.json](../../fixture/fixture.json) · Mechanics:
[design-research.md](../../design-research.md)

Starting point: OpenDesign skill
[frontend-design](../../od-pack/skill-frontend-design.md) + design system
[ds-hud](../../od-pack/ds-hud.md).

---

## 1. Metaphor in one line, and the main gesture

**A night-shift operator's instrument desk where every instrument belongs to a
machine you do not own.**

You have authority over the *wiring*, never over the *readings*. That is the
whole thesis, and it is what separates this from a cockpit HUD: in a cockpit the
panel is the aircraft's own nervous system; here each cell is a rented feed, and
the desk's job is to show honestly how fresh each feed is and to route every
decision back to the system that owns it.

**Main gesture: sweep and clear.** You read the one advisory that is coloured
and word-labelled at the top of the desk, you press the owning system's control
(the desk never performs the decision), and the fact's propagation visibly
clears the advisory out of the home cell, the service surface and the WIRE panel
at the same instant.

Anti-repeat note: HUD is on the anti-repeat list by way of "Horizon Focus HUDs".
This concept takes the ds-hud *token set and layout discipline* and rejects the
cockpit framing — no attitude/heading symbology, no ownship, no flight metaphor
for the data. See §10.

## 2. Integration model

**Where a service lives.** One cell in the instrument grid, plus two derived
surfaces. A service is never a "page" — it is one column of instruments that
Worlds composes with its neighbours.

Start state (2 connected, per the brief): `media-shelf` (healthy) and
`front-door` (needs_attention). `local-weather` is present on the desk as a
`not_configured` cell with zero facts and an empty state (the zero-provider
floor). `nightly-backup`, `garden-camera`, `studio-printer` and `notes-sync` are
offered by the ADD drawer, unconnected, using their fixture ids and states
verbatim.

**Connect lifecycle** (stage names from the brief; the contract draft #329 is
not in this repository, so field names and per-stage copy are **UNKNOWN** —
see §10):

1. **DISCOVER** — pick from the ADD drawer; the candidate's own summary is shown
   from the fixture, not invented.
2. **CREDENTIAL REFERENCE** — the form takes a *reference name only*, e.g.
   `front-door / local`. It never accepts a secret value; there is no field that
   could hold one, and the label says so in words.
3. **SAFE TEST** — an explicit "run test" button showing `RUNNING → PASSED` or
   `FAILED` in place (state-coverage's explicit-run-state pattern). No spinner
   without a timeout; the desk's test has a 6 s cap and then says
   `TEST TIMED OUT — no answer from the owning app`.
4. **PREVIEW OF FACTS/CAPABILITIES** — the fixture's facts for that service are
   listed with their own state, exactly as they will appear, with a "nothing is
   published yet" line.
5. **CHOOSE WHAT TO PUBLISH** — per-fact checkboxes. A fact left unchecked is
   never rendered afterwards, and the count of unpublished facts is stated
   ("2 facts held back").
6. **IT APPEARS** — the cell is inserted into the grid and the WIRE panel gains
   its rows, from one state change.

**How facts flow.** One in-memory store keyed by fact id, derived from the
fixture; every surface is a pure read of that store. No surface owns a copy. The
governed action (`frontdoor.back-door` → lock, using the fixture's
`crossViewExample`) writes to the store, and all three surfaces re-render from
it. The fixture is not mutated on disk.

## 3. Navigation between Worlds and a service

- **Desktop (≥1024px):** three fixed regions — a left **CELL RAIL** (service
  list, one line each, uppercase micro-label + mono state word), the **DESK**
  (the selected service's instruments, 12-col HUD grid), and a right **WIRE**
  panel (facts, source, freshness, permissions). Selecting a rail entry changes
  the desk in place; nothing navigates away.
- **Phone (390px):** one column, in this order — advisory line, cell rail as a
  horizontally *wrapping* chip row (wraps, never scrolls sideways), desk
  instruments stacked, then WIRE as the last section. Back is a real button in a
  44px-tall sticky bar, not a gesture.
- **Keyboard:** rail entries, instruments, panel rows and every action are in
  DOM order and reachable by Tab with a 2px `#00FF41` focus outline and a
  `:focus-visible` offset. Focus moves to the desk heading when a rail entry is
  chosen with Enter (state-coverage focus rule). No pointer-only affordance
  exists.

## 4. The embedded-app frame

- A `sandbox="allow-scripts"` `srcdoc` iframe standing in for the owning app's
  own UI, drawn inside a bevelled bay with a corner label `EXTERNAL APP ·
  SANDBOXED`. Scripts are allowed so the embedded app can answer the host via
  `postMessage`; `allow-same-origin` and `allow-top-navigation` are **not**
  granted, so it cannot reach the host's DOM.
- **Two fallbacks, and the distinction is the point:**
  - *Owner fallback* — the fixture's `owner.fallback` label and `app://…` href,
    verbatim (e.g. "Open Home app" → `app://home`).
  - *Ext-app fallback* — `https://example.invalid/…` for the third-party frame,
    per the brief and the fixture's `_meta.syntheticNote`, which reserves
    `.example.invalid` for any placeholder host added later. The fixture holds
    no ext-app URL, so this one string is prototype-only and marked MOCKUP.
- The shell keeps around the frame: the desk's nav, the back control, the
  advisory line, the source/freshness caption above the bay, and a
  `LAST CHECKED` clock that keeps ticking while the frame is idle.
- The frame has a visible **offline state** of its own: a deliberate
  `SANDBOX BLOCKED` rendering with the owner link still reachable, so "the app
  could not load" is never confused with "the service has no data".

## 5. The agent/human shared-facts model

One table, one row per fact, in the WIRE panel. Columns:

| Source | Freshness | Agent | Human |
|---|---|---|---|
| `LOCAL` / `EXTERNAL` / `NONE` | `SET 14:23:58Z · 28s AGO` | `READ` / `ACT` / `NONE` | `READ` / `WRITE` / `ACT` / `NONE` |

- Source and freshness are **read from `checkedAt` at render time**, never from
  the fixture's `ageSeconds` (the fixture explicitly says a viewer should
  recompute).
- Agent and human permissions are **two independent columns**, never one
  "access" field.
- `NONE` renders as a **named, disabled control with its reason in words**, not
  hidden: `frontdoor.guest-code-used` shows `AGENT: NONE — Identifying a person
  is human-only in this fixture; the agent sees that a guest code was used, not
  who.` and `camera.human-detected-today` shows `AGENT: NONE — The agent may
  not assert a detection while the source is unknown.`
- An `ACT` permission never renders as a Worlds-side button. It renders as a
  hand-off: `DECIDE IN HOME →` pointing at the fixture's owner route. Worlds
  never decides; it routes.

## 6. States

Every surface renders all of these; none is left implied.

- **First run / zero services.** A 3-line origin block (`WORLSDESK / NO CELLS
  WIRED / Add a service from the drawer to begin`) plus the Connect lifecycle
  entry point. No demo data is shown as if it were the user's.
- **Zero providers.** `local-weather` cell: `NOT CONFIGURED` in words, plus the
  fixture's `emptyReason`, plus one hand-off link to provider settings. No
  placeholder value, no dashes, no skeleton pretending to be a fact.
- **Quiet.** `media-shelf` healthy: a low-contrast `STANDBY` treatment, values
  in `--data-secondary`, and nothing animated. Nothing moves when nothing
  changed (ds-hud: do not animate what does not signal operational state).
- **Needs you.** `front-door`: `NEEDS A HUMAN` in `--data-warning`, the
  attention reason from the fixture quoted in words, and the hand-off control.
- **Stale.** `nightly-backup`: value shown in `--data-secondary` with the state
  word `STALE`, the last-successful timestamp, and `LAST CHECKED 06:00:00Z ·
  8H 24M AGO` — age always spelled, never a bare colour.
- **Unknown.** `garden-camera`: the fact slots render as `UNKNOWN — no source
  has answered` and `NONE` appears as the source. A blank is never a state.
- **Unavailable.** `studio-printer` / `notes-sync`: cause in words
  (`job rejected: no media loaded`, `provider timed out`) plus the human-only
  retry with its attempt count, following state-coverage's retry discipline
  (immediate first retry, "last attempted Xs ago", disabled with a reason after
  three).
- **Disconnected.** Disconnecting `garden-camera` leaves a tombstone cell for
  one page: `GARDEN CAMERA — DISCONNECTED 14:31. Its three facts are no longer
  published here and nothing else changed.` The tombstone is a statement, not a
  trash icon.
- **Loading.** Every probe has a 15 s "taking longer than expected" line and a
  stop-after-60 s error, never an indefinite spinner.

## 7. Borrowed, and deliberately different

- **Borrowed from Homarr 2.0** — a tile as a live *view* over an integration
  that owns its own data; and an explicit edit mode for arrangement so the live
  view cannot be changed by accident. *Different:* the desk has no free
  arrangement at all — cell order is by urgency then name, fixed. Re-arranging a
  panel you do not own would be a lie about what matters.
- **Borrowed from Dashy** — a visible per-item status check that carries its own
  checked-at time, and configuration as a reviewable document. *Different:* the
  `NOT CONNECTED` drawer is generated from the fixture and is not editable in
  the prototype (see §10).
- **Borrowed from Home Assistant cards** — states are first-class and include
  `unavailable` and `unknown` as *rendered* states rather than a stale number;
  and actions declared as data per item (here: view / hand off / retry).
  *Different:* HA treats tap/hold as three gestures; the desk uses three
  explicit, labelled, focusable controls instead, because hold is not
  discoverable and not keyboard-reachable.
- **Borrowed from MCP ext-apps** — a sandboxed frame that talks back through
  the host, never directly to a service; the host checks permission before
  dispatching. *Different:* the desk's host-mediated call is *shown* — the
  permission column is visible in the same row as the value — rather than being
  an invisible check.
- **Deliberately not borrowed:** Appsmith's drag-to-arrange editing and its
  expression-binding editor (too much machinery for a mockup, and it would make
  the config a second source of truth).

## 8. OpenDesign starting point and the guide rules applied

**Skill `frontend-design`** — used for: step 2's "commit to one specific
aesthetic direction" (dense operational, night-light only, no light mode);
step 3's "design the real interface, not a placeholder poster" (controls,
filters, empty/error states and the full Connect form are all in scope);
step 5's density rule (this is a repeated-use tool, so it is dense, not airy);
step 6's self-review, run against the floors in §6. Its step-2 banned list is
the same as anti-ai-slop and is honoured in full.

**Design system `ds-hud`** — taken as the brand contract, no values invented
outside it:

| Token | Hex | Use here |
|---|---|---|
| `--bg` | `#0A0A0A` | desk canvas |
| `--surface` | `#111316` | cell and panel fills |
| `--border` | `#1E2328` | panel separation only |
| `--data-primary` | `#00FF41` | live values, focus ring |
| `--data-secondary` | `#7FFF00` | standby / stale values |
| `--data-tertiary` | `#5A9A5A` | grid lines, tick marks, **labels only** |
| `--data-warning` | `#FFB800` | needs-a-human advisory |
| `--data-alert` | `#FF3B3B` | unavailable / fault |

Contrast gates computed against `#0A0A0A` rather than assumed: primary 14.5:1,
secondary 15.28:1, tertiary 5.85:1, warning 11.42:1, alert 5.6:1 — all pass AA, so
tertiary is safe for the 10–12px uppercase labels (ds-hud forbids tertiary for
*readout* text; that is obeyed — it carries labels and rules, never a value).
Dark only, no light mode, flat fills with no gradients, corners 0 or 4px except
the circular reticle (ds-hud's 50% rule).

**Guide rules applied** — `color.md`: four-layer palette, one accent (there is
no accent; primary is a *data* colour), accent-use cap honoured by having zero
decorative accent. `typography.md`: exactly two faces, mono for all readouts,
sans for uppercase labels, ALL CAPS at `0.08em` tracking, three weights
(400/550/700), body ≤65ch, no `text-align: justify`. `anti-ai-slop.md`: none of
the seven sins — no indigo, no gradient hero, no emoji, no invented metric, no
left-border-accented rounded card, serif/display face is the mono itself.
`state-coverage.md`: all five states per surface plus the retry and timeout
discipline. `accessibility-baseline.md`: 44px minimum targets, visible focus,
no colour-only status. `animation-discipline.md`: 150ms state confirmation,
100ms linear for value changes, transforms/opacity only, and under
`prefers-reduced-motion` every transition is stripped to an instant state change
— **the propagation tick does not run; the same fact is instead marked with a
static 2px outline for 2s, which conveys the same information without motion.**

## 9. Signature element, hero thesis, reference lock

- **Signature element — the propagation tick.** When one fact changes, a 1px
  `#00FF41` rule sweeps each affected readout in the same frame, so the user
  sees *one event, three places* rather than three coincidences. Under reduced
  motion it becomes the static outline described above. One element, used once
  per event, no ambient loops.
- **Hero thesis — the advisory line.** The single highest-urgency fact, set at
  32px mono, top-anchored above the desk, with its age spelled in words beneath
  it. The hero is a thesis, not a picture: *"Front door: back door unlocked for
  47 minutes. A decision is required; the local app will not act on its own."*
- **Reference lock (mechanics only; `[memory]`, not verified in this session):**
  the **Garmin G1000 flight deck** — a persistent advisory that takes over the
  primary field and does not repeat elsewhere — and the **layout of a military
  C2 / ISR operator station** — many feeds, equal visual weight, the operator's
  authority deliberately absent from the feeds. Palette and glyphography are
  taken from ds-hud only; nothing is copied from either.

## 10. Risks and UNKNOWNs

- **UNKNOWN — Connect contract draft (#329).** Not present in this repository
  (grepped: no connect-lifecycle or credential-reference contract under
  `docs/`). Only the six stage names from the brief are used. Real field names,
  per-stage copy, and whether the credential reference is a named secret in a
  vault or an opaque pointer are all unverified.
- **UNKNOWN — whether any real provider would accept this embed policy.** The
  `sandbox="allow-scripts"` + `srcdoc` frame proves the mechanics, but real
  providers gate embedding with CSP `frame-ancestors` and `X-Frame-Options`, and
  none of the five references in design-research.md was verified in this
  session. The MCP ext-apps spec was itself moving fast when that note was
  written.
- **Fonts.** The stack rule forbids external fonts, so `JetBrains Mono` and
  `Inter` will usually resolve to their local fallbacks. The mono fallback
  carries the identity; the sans fallback for 10–12px uppercase labels is a
  weaker match. Whether the review machine has either face is UNKNOWN.
- **HUD vs the anti-repeat list.** "Horizon Focus HUDs" is on it. The bet is
  that an operator desk with no ownship, no heading/attitude symbology and an
  explicit no-authority thesis is a different artifact. A reviewer who disagrees
  can reject the whole concept — that is the intended consequence of an assigned
  design system.
- **`ageSeconds` freshness.** Recomputing from `checkedAt` means the desk's ages
  drift upward the longer the file sits open. That is correct behaviour but it
  will make screenshots taken minutes apart disagree. Unknown whether the
  review wants a frozen clock.
- **Not decided:** whether the cell rail should show unconnected services at all
  after a disconnect, or only during Connect. Leaning toward hiding them, which
  would reduce the empty/drawer distinction.
---
## CRITIQUE.md (by another engine)

# Critique of c589 — The Instrument Desk (DESIGN.md, not code)

Critic: a different engine from the author. Four gaps, most important first. I checked the §8 contrast figures against the tokens and they are correct.

## 1. Uniqueness: phosphor-green mono on black reads as a Pip-Boy/CRT terminal or cyberdeck, not just a "Horizon HUD" (anti-repeat list)
- **What is wrong.** The anti-repeat note in §1 argues only against the Horizon HUD item. The list also bans "Pip-Boy/CRT terminals" and "cyberdecks". The proposal is `#00FF41` on `#0A0A0A`, all readouts in monospace, uppercase micro-labels, a "bevelled bay" and a reticle. The reference lock adds Garmin G1000 and a C2/ISR station. A reviewer will see a green-on-black terminal, however the thesis is worded. The thesis ("authority over wiring, not readings") is not visible in the look.
- **Fix.**
  - Keep the ds-hud tokens but make the tokens carry less of the look. Drop the bevelled bay and the reticle.
  - Keep `--data-primary` for the focus ring and for values that change, and use `--data-secondary` or foreground for static text.
  - Make the thesis visible structurally instead. A persistent split in every cell between "owned elsewhere: reading" and "yours: wiring" would do it. At present it exists only in prose.

## 2. The governed action has no actor, and the "disconnect" demo contradicts the start state (req 5, req 6)
- **What is wrong.** §1 says "you press the owning system's control (the desk never performs the decision)" and the advisory then clears. §2 says the action "writes to the store". Where is that control? If the lock is a button on the desk, Worlds is deciding. If it is in the embedded frame, §4 never says the frame sends the message or how the host validates it.
- **What is also wrong.** §6 disconnects `garden-camera`, but §2 says that service is only in the ADD drawer at start. The user must connect it first, and the design does not say so. The tombstone is also "for one page", which is ambiguous. Does it vanish on the next navigation? Req 6 says Worlds says "plainly what is gone".
- **Fix.**
  - Say that the lock happens inside the embedded Home frame, which sends `postMessage` (host checks origin and permission). The desk only shows the result and offers the `app://home` hand-off.
  - Disconnect a service that is connected at start (`front-door` or `media-shelf`), or say the demo needs a prior connect.
  - Keep the tombstone until dismissed, and print the removal time and fact count.

## 3. The hero is hardcoded time-sensitive prose, and a ticking "LAST CHECKED" clock may imply liveness (honesty floor, req 4)
- **What is wrong.** The advisory *"Front door: back door unlocked for 47 minutes."* is copy. The fixture's 47 is the value at `checkedAt` 14:24:02Z. It goes stale on screen with no wording change. The "LAST CHECKED clock that keeps ticking while the frame is idle" (§4) is ambiguous. A clock that ticks beside a fact that has not been re-checked looks like a live reading.
- **Fix.**
  - Compose the advisory from the store as "47 min as of 14:24:02Z, checked Ns ago".
  - Make the ticking element visibly an age counter ("N s since check") and not a clock.
  - Pick the hero by an explicit rule (highest state severity, then name), and say what it shows when two facts need a human.

## 4. Phone layout buries the shared-facts view, and ds-hud's 10px labels sit below practical legibility (req 4, floors)
- **What is wrong.** On phone, the WIRE panel is the last section, below the stacked instruments. With 7 services, the rail chips (44px, wrapping) may push the desk off the first screen. The propagation tick (§9) sweeps "each affected readout in the same frame", but on phone those readouts are in different screens' worth of scroll. The "one event, three places" signature cannot be seen. The 10–12px uppercase tertiary labels are the DS's own sizes and still weak at 390px.
- **Fix.**
  - On phone, show the changed fact's WIRE row inline in the advisory region. Alternatively, add a sticky "WIRE: 1 changed" bar that jumps to the row.
  - Show only connected services in the rail, and move the rest to the ADD drawer.
  - Set labels at 12px minimum. Raise it on phone and note the deviation, as the design already does for other DS issues.
