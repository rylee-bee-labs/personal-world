# Build the full Worlds prototype from this design spec (MOCKUP)

Rylee: "comprehensive but also keep the original feeling" (protect the look, the feel, Dot, the warmth and delight; copy may change). Build ONE self-contained HTML file (inline CSS and vanilla JS; no network, no external fonts, images or libraries) that implements the WHOLE flow in the spec below, not a single page: welcome with Dot (#skip-welcome) -> counter -> connect (every step, including a failed test and retry) -> composed home -> a service's surface -> embedded app (loaded, and refused with fallback link) -> #simulate-change updates every view at once -> Needs You handoff that is fail-closed ("approvals aren't connected yet") -> inert proposals -> stale/unknown arriving via the live feed (#live-toggle pauses it) -> revoked credential -> #disconnect (visible on the first screen for the selected service; confirm button labelled "Disconnect") -> back to empty. Desktop 1440, tablet 768 and phone 390 layouts designed on purpose. Use the house tokens (Rylee-Bee Design System, Mulberry & Mint, dark by default, soft contrast). Keyboard reachable with visible focus, 44px targets, prefers-reduced-motion, stale/unknown in words, visible MOCKUP label. Use V2 (below, the owner's pick) as the visual starting point: keep its look and feel and Dot.

## The spec

# Worlds vNext: the full design (MOCKUP spec)

Status: MOCKUP. This is a design, not an approved product and not deployed. It describes screens and behaviour; it contains no code. Data is the synthetic `docs/vnext/fixture/fixture.json` only. Where this spec shows something the fixture does not contain, it says so and marks it as a sample or UNKNOWN.

## 1. The feeling to protect

Rylee's words: "comprehensive but also keep the original feeling", clarified as protect the feel, the look, the whimsy and the love, **not the copy**. This section is a review criterion for every later section. If something added below makes Worlds look like a generic admin console, it is wrong, however complete it is.

Sources studied: V2's `DESIGN.md` and `index.html` (the look as built: Mulberry & Mint), and Concept 5's `DESIGN.md` and `index.html` (where Dot, the stamp and the parcel were first drawn). Concept 5 was skinned in Wise lime and heavy type; that skin is **not** what Rylee picked. V2's Mulberry & Mint skin is. Where the two disagree, this spec follows V2's look and Concept 5's gestures.

### What makes it feel the way it does

**The look**

1. **A plum room with mint and apricot in it.** Everything sits on the three plum surfaces (`--surface-base` page, `--surface-raised` slips and cards, `--surface-sunken` wells and rails). Mint (`--action-primary`, `--accent-mint`) is the one thing to act on. Apricot (`--accent-apricot`, which is also the "needs you" colour) and lilac are accents, never fills. Seen in V2: the top bar, rail, slips, receipt, and the primary pills.
2. **Soft, round, paper-like shapes.** Slips and cards use `--radius-lg`; buttons and chips are full pills (`--radius-full`); inner wells use `--radius-md`. The receipt has a 2px `--border-line-strong` frame like a printed slip; empty states use a dashed `--border-line-strong` outline, like a space waiting for something. V2: `.slip`, `.receipt`, `.empty`, `.welcome`.
3. **Depth by surface, not by effect.** No gradients, no glow, no heavy shadows. Depth is the surface ladder plus a small lift: a slip rises about 2px and its border turns mint on hover or selection (150ms). Shadows exist only on things that float: the phone add-service button and the toast. V2: `.slip:hover`, `.fab`, `.toast`.
4. **Young Serif for the voice, Atkinson Hyperlegible Next for the reading, and a real size step between them.** The serif on headings gives the counter a printed, hand-labelled air; most of that air is the jump in size between the display face and the reading face (V2 built about 2.5:1 at page level). The body face keeps it readable. V2: every `h1`/`h2`/`h3`. §6 keeps this step with two product-local display handles.

**The counter and the slips**

5. **Worlds is a service counter.** "Your counter" holds one **slip** per service: name, kind, state word, one-line reason, one peeked fact, "Checked 24s ago". You pick a slip and open its **receipt** (Source → Worlds → Agent / Human), three small sunken stations inside a framed card. That receipt is the daily hero. V2: `.slips`, `.receipt-stations`; Concept 5: `.st` stations.

**Dot, and how Dot behaves**

6. **Dot the snail is a small accent, never a gate and never the data courier.** Dot is drawn in line art with a spiral shell and eyestalks (Concept 5 `dot()`; V2's flatter apricot-shell, mint-body version). Fixed rules, taken from both concepts:
   - **Asleep** (closed eyes, a small "z") when the counter is empty: first run, after the last service is disconnected, empty lists. Concept 5 `dot('asleep')`, V2 §6.
   - **Nods once** (200ms) beside "Updated just now" when a service's state word changes. Never on a plain tick. Concept 5 `.dot-nod`.
   - **Carries the parcel** across when a service is published (400ms desktop, 300ms phone). V2 `dot(true)` carries a slip; Concept 5 `dot('parcel')` carries a ribboned parcel.
   - **Rests** beside the freshness line in the header of every working screen (Home, Connect, Memory, Settings), still.
   - Dot's face and pose never say whether something is good or bad. The words say that.

**The delight moments (the whimsy and the love)**

7. **The "Connected" stamp.** A tilted (about -4°) stamp in Young Serif capitals with a check lands beside Dot when a connection completes. Concept 5 `.stamp`, kept in V2 §11. It is the one flourish allowed to be a little proud.
8. **The parcel hand-off.** The Publish step is not a spinner; Dot walks the parcel to the counter and the new slip appears. Concept 5 `phase === 'delivering'`.
9. **The quiet calm.** When nothing needs a person the counter says so in one plain line and Dot rests. No confetti, no streak, no score. V2 `home()` status line.
10. **The receipt settling.** When a fact changes, the three stations tick in order, Source then Worlds then Agent / Human, 60ms apart. It is the counter showing its work. V2 `.station.updated`, Concept 5 §15.
11. **Kind small print.** Care shows up as a provenance line ("Connected during first run · reference held, never the secret"), as the service-drop line, and as "Worlds locked nothing." They sound like a person who understands what is happening wrote them. The wording is free to change; the habit of saying them is protected.

**The pacing**

12. **An unhurried, skippable welcome, one thing at a time.** V2 opens on a single broad welcome card with a sleeping Dot, one invitation and one button, with "Skip welcome" always available; it earns its way to the counter one connection at a time, with Dot carrying each slip across. Nothing is a tour you must finish.

**The motion character**

13. **Gentle, one curve, and only to confirm.** One easing, `cubic-bezier(.2,0,0,1)`. Motion confirms a change that has already happened; nothing moves on a routine tick except the age number. Under reduced motion the same moments become opacity-only or instant, and data never stops. V2 §11.

### How to use this section
- **Copy is not protected.** Every string in §7 is rewritten for clarity using `docs/vnext/house/LANGUAGE.md` (machine truth, then plain words, then optional voice).
- **Coverage must not flatten the feeling.** §4.0 carries a per-screen table naming which of items 1-13 each screen keeps, so the rule can be audited. Every working destination (Home, Connect, Memory, Settings) names at least one gesture from items 5-13 beyond the shared look. Where a screen is consequential (Needs you, disconnect confirm, revoke confirm, reference refused) it is deliberately *quiet*: no Dot, no flourish. `JOY-CURIOSITY-AND-CARE.md` says operational and consequential surfaces may be very quiet, and gifts are not toll booths.
- **Ignorable.** A person can ignore every flourish (Dot, stamp, parcel) and finish every job.

## 2. What this design is

One Worlds, one flow, from an empty counter to a counter that keeps working: welcome, connect a service, preview it, publish it, watch facts change and age, follow a link out to the owning system, disconnect, revoke, and be back at an honest empty counter.

**Chosen base: V2 "Welcome-led Counter"** (Rylee: "V2 is the only one that passed"), grown to the full application using the Field Guide structure, the Rylee-Bee house tokens (Mulberry & Mint), and the Connect contract with Sol's six pre-prototype conditions.

### Deliberate changes from V2 (so reviewers can find them)
| # | Change | Why |
|---|---|---|
| 1 | Connect is a **destination page** (desktop, tablet) and a full-screen stepper (phone), not a modal dialog. | Back-button, deep links and focus order. V2 and Concept 5 already agree Connect is a destination in the nav. |
| 2 | **Needs you** is its own destination. | The Field Guide skeleton names it; it is also where the Project Home handoff lives (Sol condition 1). |
| 3 | The line "Mock handoff recorded; Worlds locked nothing." and the "Hand off to Home" button are replaced. | "Handoff recorded" reads as if an approval path works. It does not yet (condition 1). Replacement: an **owner link** ("Open Home app") and a separate **approval handoff** that honestly says "Approvals aren't connected yet." |
| 4 | **Disconnect keeps the credential reference** and has no tick to remove it. Removing the reference is a separate **revoke** with its own confirmation. | The Connect contract separates `disconnect` (reference retained) from `revoke` (config deleted, held credentials purged). V2 deleted the reference on disconnect. |
| 5 | Status chips say **"Needs you"** (LANGUAGE.md human label for `needs_attention`) where V2 showed the fixture `stateLabel` "Needs a human". | LANGUAGE.md owns the human labels; the fixture's `attentionReason` is still shown verbatim. |
| 6 | Page and section titles use two Worlds-local display handles composed from `--type-h1` (§6); no `clamp()`, no loose `calc()` in the build. | The house says a heading ladder past `h1` is a product's typographic decision composed from the shared tokens (`house/DESIGN.md`). V2's *source* used untokened `calc()` multiples of `--type-h1` where its design asked for a composed handle; this spec keeps V2's size step and names the handle. |
| 7 | Discover can name **every** not-yet-connected fixture service, shown honestly as "Not connected yet · no reading". | The proof path needs stale and unknown reached through a service the person actually connected. |
| 8 | The receipt is per **fact**, with a fact list above it, not only the first fact. | V2 showed `facts[0]` only. |
| 9 | The welcome is V2's single broad card, then earns **two** connections one at a time, with the parcel each time; the fact-journey is a note reached after the first connection. | Keeps V2's named onboarding signature, and makes "Connected during first run" true of something the person watched. |

### Inputs honoured
`docs/vnext/full-design-brief.md`; V2 and Concept 5; Field Guide docs; house tokens and `LANGUAGE.md`; the Connect contract `docs/vnext/connect/connect-contract.md`; `concept-brief-v2.md` requirements 1-6 and `concept-brief-v3.md`; the fixture. Rylee's standing preferences: dark by default with soft contrast, reduced motion respected, full keyboard support, 44px targets, plain words.

## 3. Information architecture

Worlds is the **Experience** layer: a costume over the stable skeleton. Platform contracts (the Connect contract) and Capabilities sit beneath it and do not change when the costume does ("Stable meaning. Mutable form."). The skeleton's four doors stay recognisable; Worlds gives each a counter-and-Dot form.

| Worlds destination (nav label) | Field Guide door | What lives here | Why it sits there |
|---|---|---|---|
| **Home** ("Your counter") | **Home**: orientation, what matters now | Status line, service slips, selected service, its facts and receipt, its app frame. The welcome lives here until done or skipped. | Home answers "what matters right now?" The counter is the Worlds form of that. |
| **Connect** | **Explore**: looking around, trying, synthesising | Discover, reference, safe test, preview, choose, publish. | Connecting is exploring what a service offers before committing. See Q1: the Connect *contract* is Platform/Capability; only this screen is Experience. |
| **Needs you** | **Needs You**: consequential human decisions | Services that need a person, owner retries, inert proposals, and the handoff to Project Home. | Authority stays with the human via Project Home's boundary. Worlds shows and links; it never decides. |
| **Memory** (page title "Agent facts") | **Guide**: understanding and continuity | Every shared fact from every connected service, as a **trail**: each service heading links to its owner, each fact says where it came from, who can see and act, and, for Unknown, what would help. | The Guide door's job is to leave a followable path. |
| **Settings** | not a door; a utility reachable from all four | Connected services, credential references, providers, live feed, motion and appearance, about. | Settings is about the person and the install. On desktop and tablet it sits apart at the rail's foot. On phone it is **not a tab**; it is reached from the page header of every screen (§5.3), so the four doors stay the whole bar. |

Rules:
- **Skeleton words stay out of the way.** Nav labels are Worlds words and plain words. The mapping above is for the team and the Field Guide link in About.
- **Dropping the costume must not break the product.** Every destination works with Dot, the stamp and the parcel removed.
- **A service is not a destination.** A service's surface is a *view* inside Home (and linked from Needs you and Memory). It is opened by choosing its slip.
- **Order of nav:** Home, Connect, Needs you, Memory, then Settings (rail foot on wide screens; header link on phone). "Needs you" shows a count badge with its number in text (for example "Needs you, 1").

### Deep links (prototype routes, for back button and focus)
`#/welcome/1-3`, `#/home`, `#/home/{service-id}`, `#/home/{service-id}/{app}`, `#/connect/{step}`, `#/needs-you`, `#/memory`, `#/settings/{section}`. Browser Back moves one screen; focus returns to the control that opened the screen.

## 4. Every screen and state

Conventions: **Carries** names which protected-feeling items (§1) a screen keeps. State words use the closed vocabulary mapped in LANGUAGE.md: Healthy, Needs you, Stale, Unavailable, Unknown, Not configured (and Working, Done, Failed for the connect test). A state always has a word; colour, glyph and Dot are supplemental.

### 4.0 Screen inventory and what each carries

| ID | Screen | Route | States covered | Carries (items from §1) |
|---|---|---|---|---|
| W1 | Welcome card | `#/welcome/1` | default, skipped | 1, 2, 4, 6 (asleep), 12 |
| W2-W3 | First and second connection | `#/welcome/2-3` | default, skipped, parcel | 5, 6, 7, 8, 11, 12 |
| H0 | Empty counter | `#/home` | first run, after skip, after last disconnect | 1, 2, 4, 6 (asleep), 9 |
| H1 | Counter, composed | `#/home` | quiet, needs-you, stale, unknown, unavailable, offline | 1-6, 9, 10, 13 |
| C1 | Connect: Discover | `#/connect/discover` | empty list, candidates, already-connected | 1-5, 6 (rests), 11 |
| C2 | Connect: Reference | `#/connect/reference` | pristine, dirty, valid, refused-secret, refused-own-secret, pending | 1, 2, 11 |
| C3 | Connect: Test | `#/connect/test` | idle, running, passed, failed, retry n of 3, limit | 6 (static, awake), 11 |
| C4 | Connect: Preview | `#/connect/preview` | loading, facts, empty | 5, 6 (static), 11 |
| C5 | Connect: Choose | `#/connect/choose` | all, some, last-one-locked | 2, 11 |
| C6 | Connect: Publish and Connected | `#/connect/publish` | confirm, delivering, done | 5, 6, 7, 8, 10, 13 |
| S1 | Service surface | `#/home/{id}` | facts, app, agent view (phone tabs) | 1-5, 10, 11 |
| S2 | App frame | within S1 | loading, loaded, refused, timeout | 2, 11 (quiet otherwise) |
| N1 | Needs you | `#/needs-you` | items, owner retries, sample proposal, empty | 1, 2, 11; **quiet**: no Dot on items; Dot rests on the empty state only (6, 9) |
| N2 | Approval handoff | sheet or panel | not available (default) | 11; **quiet** |
| D1 | Disconnect | dialog or sheet | confirm, working, done | 11; **quiet**; Dot falls asleep only when the counter becomes empty (6) |
| R1 | Revoke | dialog or sheet | confirm, working, done | 11; **quiet** |
| M1 | Memory (Agent facts) | `#/memory` | rows, empty, filtered | 1, 2, 5 (fact rows), 6 (rests in header), 9, 10 (rows settle when a change lands), 11 |
| T1-T5 | Settings sections | `#/settings/{section}` | each with loading, empty, error | 1, 2, 6 (rests in header beside the live line), 11 |
| X1 | Offline, error, loading | overlays | offline strip, failed to load, saving error | 11, 13; Dot rests |

### 4.1 Welcome (W1-W3) and the first run

First run opens on Home with the welcome card in the main area; the shell (top bar, nav) is already there, so nothing is hidden and "skip" is genuinely a skip. `#skip-welcome` is on **every** beat and goes to H0 (or, after a connection, to the counter as it stands).

| Beat | What it shows | Primary action | Carries |
|---|---|---|---|
| **W1 Welcome** | V2's one broad raised card in a sunken well: **sleeping** Dot, the heading in `--wv-display-page`, two plain sentences about what Worlds is, one invitation. | "Connect a service" (to W2) | 1, 2, 4, 6, 12 |
| **W2 First service** | A compact connect for **Media Shelf** (the first service). Shows its name, kind and owner; the scope sentence ("Worlds will send this reference only when reading: …") with its capability ticks already on; a static one-line test result ("Passed. Nothing on Media Shelf was changed."); then one button. Dot wakes, carries the parcel (400ms desktop, 300ms phone), and the **Connected** stamp lands beside Dot. | "Connect Media Shelf" | 5, 6, 7, 8, 11 |
| **W3 Second service** | The same compact connect for **Front Door**. After the second parcel, the card closes and the counter appears with both slips; Front Door is selected. | "Connect Front Door" | 5, 6, 7, 8 |

The welcome's connections write the same records a full Connect run writes, with the reference handles `ref://media-shelf` and `ref://front-door` (labelled mock handles, **scoped**) and the provenance line "Connected during first run · reference held, never the secret." No real network is called, and each beat says so.

**After the first connection**, the first time a receipt is opened a dismissible note (not a beat, never blocking) draws "How one fact travels": Media Shelf, "Items in library: 1,284", as the three stations Source (Media Shelf, local), Worlds (shared as "Items in library"), Agent / Human. One sentence: every fact says where it came from and who can act. It carries 5 and 10 and is reachable again from Settings > About.

Rules: no countdowns, no auto-advance; "Back" from W2 and W3; the welcome is never shown again after finish or skip (stored in the mock session; **Settings > About > Replay the welcome** brings it back). Skipping on W2 or W3 leaves whatever was already connected in place. Reduced motion: no Dot movement; the parcel and stamp appear in place.

### 4.2 Empty counter (H0)

Shown after skip, and again whenever the last service is disconnected. This is "back to empty".

- Dot **asleep** in a dashed-outline well; heading "The counter is quiet"; one sentence; `#add-service` (also in the top bar).
- Status line: "No services are connected yet."
- Selected-service strip: "No service selected." with `#disconnect` visible and **disabled** (`aria-disabled`), with the reason "Connect a service before disconnecting" as visible text, and `aria-describedby` pointing to it.
- Local Weather is **not** shown as a slip. It has no facts and is not connected; it appears in Settings > Providers as Not configured.
- Memory shows its own empty state; Settings shows empty lists (same sentence).

**Carries:** 1, 2, 4, 6 (asleep), 9.

### 4.3 Home: the counter, composed (H1)

**Initial settled set:** after the welcome, Media Shelf (Healthy) and Front Door (Needs you), `media-shelf` and `front-door`. Front Door is selected on load, so `#disconnect` reads "Disconnect Front Door" on the first screen after the welcome. If the welcome was skipped, the counter starts empty (H0) and there is no settled set.

Order (reading order and DOM order are the same):
1. Page header: "Your counter" (`--wv-display-page`), with Dot resting beside the freshness line.
2. **Status line:** "1 needs you · checked 24s ago", with the live and simulate controls (see §5 for where they sit per width).
3. **Slips** (one per connected service). Each: name; kind; state chip (glyph + word); the fixture **summary** or `attentionReason` as the reason line; one peeked fact (label and value, or "No facts"); "Checked 24s ago". Selected slip shows the word "Selected" and a check shape plus the mint border (never colour alone).
4. **Service detail**: header (name, state chip, owner app, `#disconnect`), **fact list**, the **receipt** for the selected fact, the **Agent sees** line, then the **app frame** (secondary).
5. Dot at rest beside the freshness line, and "Updated just now" with a nod when a state word changes.

State presentation on a slip and in the detail. **The fixture's own sentence is always shown**; any Worlds explanation goes *under* it, never in its place.

| State | Chip | Slip reason line (fixture text, verbatim) | Worlds' added line | Value shown |
|---|---|---|---|---|
| Healthy | check + "Healthy" | `summary` | none | the value |
| Needs you | alert triangle + "Needs you" | `attentionReason` first, then `summary` | none | the value |
| Stale | clock + "Stale" | `summary` (for Nightly Backup: "Last successful run is older than the configured daily window.") | "Nothing has changed since {time} that Worlds knows of." (for a simulated stale, where there is no fixture reason, this line is the reason) | last known value, labelled "last known" with its checked time |
| Unavailable | x in circle + "Unavailable" | `summary` | per-cause line (§4.9) | "Unavailable" for a fact with no value; last good value kept and labelled |
| Unknown | question mark + "Unknown" | `summary` and `unknownReason` | "Worlds has no current reading for this and won't guess." | "Unknown" (never blank, never 0) |
| Not configured | dash + "Not configured" | `summary` | none | none; no placeholder |

**Quiet.** When the attention count is 0 the status line reads "Nothing needs you right now." Healthy and current are said in words, with no celebration. After `#simulate-change` with nothing else outstanding: "That was the last one. Nothing needs you right now." **Carries:** 1-5, 9, 10.

### 4.4 The Connect lifecycle (C1-C6)

Connect follows the contract's verbs in the order the person meets them: **Discover → Reference → Test → Preview → Choose → Publish**, then the **Connected** moment. It is one page with a step list (vertical on desktop, a "Step 3 of 6 · Test" line on phone). Every step has Back; Cancel returns to Home without changes; nothing is saved until Publish (discover and test write nothing; the contract's `register` and `publish` are what persist). No real network is called in the mock, and every step that would call one says so.

**Two different ticks, two different contract fields.** Step C2 binds the credential's **send scope**: which *capabilities* the reference may be sent to (the contract's `CredentialReference.scope`, checked before Worlds dials). Step C5 sets the **publish selection**: which *facts* appear on the counter and to agents. They are never the same list and each screen says which it is.

**Proof-path services.** Discover can list: Notes Sync (the default first stop), Nightly Backup, Garden Camera, Studio Printer, plus anything the person disconnected or revoked. Local Weather is not offered: it is Not configured and belongs to Settings > Providers. Services already at the counter are listed under "Already at the counter". Discover shows **no state word and no reading** for a candidate, because Worlds has not read anything yet.

| Step | What the person does | What Worlds says it will and won't do | Carries |
|---|---|---|---|
| **C1 Discover** | Picks a service from the candidates. Each shows name, kind, owner app, "Not connected yet · no reading", and the capabilities it is said to offer, each marked "not verified yet". Authentication need reads "Auth: unknown" (never a silent "none"). | "Looking only. No reference has been used yet. This sends no credential and changes nothing." Candidates are proposals, not facts: each shows who says it exists. | 5 |
| **C2 Reference (send scope)** | Enters a **credential reference**, not a secret, and ticks which **capabilities** the reference may be sent to (one checkbox per discovered capability, at least one, default all). | The reference is checked for shape when the person leaves the field. Worlds shows the scope sentence *before* any test: "Worlds will send this reference only when reading: {capability list}." plus "This is about what the reference can be used for. It is not what appears on your counter." Next stays disabled until the reference is valid and at least one capability is ticked. | 11 |
| **C3 Test** | Presses "Run safe test". It cannot run without a valid, scoped reference. | One read, nothing changed on the owning service. The reference is sent only within scope. Static progress ("Testing…", no spinner). Passes first time by default; a labelled mock checkbox "Simulate a failed test (mock)" makes it fail. | 6 (static Dot, awake) |
| **C4 Preview** | Reads what the service offers, now actually read. | Facts appear with their own states and are never merged: for Notes Sync, "Provider reachable: Unavailable", "Local notes waiting to upload: 3 notes, Live", "Last successful sync: Stale". Capabilities now read "verified". | 5 |
| **C5 Choose (publish selection)** | Ticks the **facts** to show on the counter. | At least one must stay ticked. Plain sentence: "Shown on your counter and visible to agents: {list}." Anything unticked is kept out and named later as "kept out". | 11 |
| **C6 Publish** | Presses "Publish". | Shows the summary, then Dot carries the parcel (400ms desktop, 300ms phone), then the **Connected** stamp lands and the slip appears. Offers "Open {service}", "Add one more", and "Manage this reference in Settings". | 7, 8 |

**C2 Reference field states** (pristine, dirty, valid, refused, pending):
- **Pristine:** "Not edited yet. Use the form ref://name."
- **Dirty:** "Edited. Worlds checks it when you leave the field."
- **Valid:** a check shape and "Looks like a reference."
- **Refused (looks like a secret):** anything not shaped like a reference is refused. The field is **cleared at once**, the secret is never put into any message, announcement or stored state, and the message says nothing was saved. This answers the contract's "no secret value round-trip into the browser". This state is quiet: no Dot, no flourish.
- **Refused (Worlds' own secret):** a reference to one of Worlds' own secrets (names beginning `PW_`, `OIDC_` or containing `BOOTSTRAP`) is refused with its own sentence.
- **Pending:** "Saving reference…" static, field read-only.

**C3 Test states**: idle, running (static), **passed** (what was read, how long it took, "Nothing on {service} was changed"), **failed** (specific reason from the fixture `lastError`, for example "provider timed out"; the reference stays in the field; "last tried {n}s ago"; "try {n} of 3"; "Try again"; recovery text), and **limit reached** after three tries ("Run safe test" disabled with `aria-disabled` until the reference is edited). The failure never blames the person.

**C4 Preview states**: loading (static Dot, "Dot is fetching the facts…", with a link to the owner fallback if it stalls), facts, and empty ("This service offers no facts yet. Nothing is guessed.").

**C6 outcomes.** Optimistic: the new slip is in the counter behind the Connected card the moment Publish is pressed; the parcel and stamp settle it. Afterwards the service's receipt says, in its Worlds station, "Connected {when} · reference held, never the secret." A failed Publish removes the optimistic slip and says why. The first fact published through Connect is also the one that is armed to go Stale then Unknown (§9), so the proof path can be walked on a service the person connected.

**Leaving the flow:** Cancel, browser Back and Escape on the step page ask nothing when nothing was saved; the entered reference is dropped.

### 4.5 A service's surface (S1)

Inside Home, with Worlds navigation, the MOCKUP label and a back path visible around it. On desktop it is the right-hand detail pane; on phone it is a pushed screen.

Order: **header** (name in `--wv-display-page`, kind, state chip, "Owned by Home app", freshness, `#disconnect`) → **fact list** (compact rows; choose one) → **receipt** (the hero) → **Agent sees** line → **owner retry** (where the fixture offers one, §4.8) → **app frame** (secondary strip) → **kept out** note if the person left facts out in Choose ("1 fact is kept out of Worlds. You chose not to share it.").

**The receipt (daily signature).** Three stations in a row (stacked on phone):
- **Source:** who produced it ("Home app reported **unlocked**"), source word (local, external or none), state word, "Checked 24s ago".
- **Worlds:** "Shared as Back door." The fact id, freshness word, and the provenance row ("Connected during first run · reference held, never the secret", or "Connected {when} · …"). Also the reference's **send scope** word: "Scoped to {n} capabilities" or "Unscoped" (see §10).
- **Agent / Human:** two separate labelled lines, **Agent can** and **Human can**, each with its action word in plain terms (see §7), and the fixture reason where there is one. A `none` permission is a **named, focusable, disabled control** carrying its reason (`aria-disabled`, not removed).

When a change lands, the stations re-settle in order Source → Worlds → Agent / Human, 60ms apart, with a tick at each, and "Updated just now" (with Dot's nod) appears for 3s.

**Fact list rows.** Label, value (or Unknown / Unavailable), state word, "observed {time}". For Front Door: Back door (unlocked, Human can act, reason shown), Unlocked for (47 minutes), Guest code last used by (visitor-3, Agent can't see it, reason shown). For a service with no facts (Local Weather, if ever shown): no rows and the fixture `emptyReason`.

**Phone:** in-page tabs **Facts** (list and receipt), **App** (frame and fallback), **Agent view** (that service's rows as in Memory). Facts is selected on arrival.

### 4.6 The embedded app frame (S2)

A bordered, labelled frame ("Home app, shown inside Worlds") holds the owning app, below the receipt. The fixture owners and fallbacks are used exactly: "Open Media app" → `app://media`, "Open Home app" → `app://home`, "Open Memory app" → `app://memory`, "Open Provider settings" → `app://settings/providers` (Local Weather). In the prototype an `app://` link cannot navigate; it opens a small note "This would open {app} outside Worlds" so no dead link exists.

| State | What shows | Controls |
|---|---|---|
| **Loading** | Static Dot and "Dot is fetching the app…" in the frame; the fallback link is already present. | Fallback link; Cancel |
| **Loaded** | The contained app preview. Frame header names the owner app and the state word. Text under it: "This app doesn't receive Worlds' credentials." | Fallback link; Reload |
| **Refused** (the app can't be shown inside Worlds) | The frame body is replaced by: "This app didn't load inside Worlds." plus the cause if known ("The app doesn't allow being shown inside another page") and "Use the link below, or try again." The slip, receipt, facts and state stay exactly as they were. | "Try again", "Open {app}" |
| **Timeout** | Same as refused, cause "It took too long to answer." | same |

The frame never receives Worlds credentials, and any consequential action inside the app belongs to that app. Real sandbox rules and browser support are UNKNOWN (Q4). Worlds navigation, back and the status line keep working with the frame in any state.

### 4.7 Needs you (N1) and the Project Home handoff (N2)

**Needs you is where Worlds is quietest.** No Dot on item cards, no flourish. Dot rests only on the empty state.

**N1 items.** One card per service that needs a person (`needs_attention`). Front Door's card: name; chip "Needs you"; fixture summary ("Back door has been unlocked for longer than the person's usual window."); fixture `attentionReason` ("A decision is required; the local app will not act on its own."); the facts behind it; and **two separate routes**, never merged:
1. **"Open Home app"**: the owner link (`app://home`). This is where the person decides. Always available.
2. **"Ask Project Home to review"**: the approval handoff. It is a link or handoff, never a button that "does" something. In this mock it is **fail-closed**: its state is "Not available yet", shown at all times in text beside it.

Services that are Unavailable and offer an owner retry (Notes Sync, Studio Printer) are listed below as **owner retries** (§4.8), not as approvals.

**N2 the handoff panel (fail-closed, the only state built).** Opening "Ask Project Home to review" shows, in a panel (sheet on phone):
- Heading: "Approvals aren't connected yet."
- Body: "Worlds can't send this to Project Home yet, so nothing was sent. You can decide in the Home app instead."
- Buttons: "Open Home app" (primary), "Close".
- A line stating the intended path: "When approvals are connected, this will open Project Home with the proposal. Project Home, not Worlds, makes the decision."
- Never shown: "Sent", "Pending approval", "Approved", a spinner, or a recorded-handoff receipt.

**Design note for later (not built, not reachable).** A future "Link ready" state would say what will be opened, in which system, and that Worlds records no decision, with a real link. It must be *proven* connected; any other state, including "unknown whether approvals are connected", renders as the fail-closed state above. The prototype has no control, setting or UI path to this state (§10, §11).

**N1 empty:** Dot rests; "Nothing needs you right now."

### 4.8 Owner retries and inert proposals

Two different things, kept apart.

**Owner retries (fixture-true).** The fixture gives Notes Sync "Check provider now" (`probe-provider`, `allowed: human`, attempts 2, last error "provider timed out") and Studio Printer "Retry last job" (`retry-last-job`, `allowed: human`, attempts 1, last error "job rejected: no media loaded"). Worlds shows them as text with the attempt count and last error, and one route: a link to the owning app ("Open Memory app", "Open Studio app" using the fixture's owner link). **There is no Run or Retry button in Worlds.** The retry happens in the owning app, by a person. They are not approvals and are not proposals.

**Proposals (contract shape; the fixture has none).** The fixture contains no proposal objects, and this spec does not invent fixture-labelled ones. The proposal component is drawn from the contract's `ActionProposal` (`exposed: false`, effect, authority) and appears in the prototype **once, as an illustrative sample** on Needs you, clearly marked "Sample. The fixture has no proposals." Its fields: label "Sample action"; effect word "Changes something outside Worlds"; "Proposed. Not active."; "Decided by: not known yet" (bound to the contract's `authority`, shown as UNKNOWN rather than choosing Home or Project Home); and "Agent can't do this."
- **No Run, Do it or Approve control exists on a proposal.** The only controls are "Open Home app" and "Ask Project Home to review" (§4.7). A proposal row is read as text, not focusable as a command.
- `lock-back-door` appears only in its applied form (§9: the owning app reports it done). Worlds never shows it as a pending proposal.
- No agent can propose, adopt or run anything; an adopted external write is not an agent tool (`exposed: false`).

### 4.9 Stale, unknown, unavailable, reference refused, not configured

These are **states of the same screens**, not separate pages. Each keeps the last-known value visible and labelled, says when Worlds last checked, offers a route to the owner, and never promises recovery.

| State | Cause (mock) | What the person sees | What they can do |
|---|---|---|---|
| **Stale** | Fixture: Nightly Backup. Scripted: a live fact misses a re-check (§9). Real rule is UNKNOWN (Q6). | Chip "Stale" (with "(simulated)" for scripted ones). Fixture summary first; then "Nothing has changed since 2:23 PM that Worlds knows of." Last value labelled "last known". Receipt Source station: "Last checked 3m ago." | "Check now" (restores the fact to live in the mock, only for scripted staleness), "Open {owner app}". |
| **Unknown** | Fixture: Garden Camera. Scripted: a stale fact left for 60 more seconds becomes Unknown (simulated). | "Worlds has no current reading for this and won't guess." plus fixture `unknownReason`. Value "Unknown" (never blank). Any last known value in its own labelled line. Agent can't assert (fixture reason). | "Open {owner app}". No retry is invented. |
| **Unavailable, provider silent** | Fixture: Notes Sync. | "That service stopped answering. Its last facts are still here, marked with when Worlds checked." Fixture summary and last error. | "Open Memory app" (owner retry, §4.8). |
| **Unavailable, job rejected** | Fixture: Studio Printer ("The last job failed; the printer is reachable but rejected the job."). | The fixture's own sentence, then "The printer answered, so Worlds is not treating it as silent." Last error "job rejected: no media loaded". | "Open Studio app" (owner retry, §4.8). |
| **Reference refused** | The provider stopped accepting the reference (mock trigger in About). | Chip "Unavailable", reason "Notes Sync stopped accepting this reference." Last facts kept and labelled stale. The error is classed, not free text from the provider. | "Replace reference" (opens Connect at Reference, field empty), "Open Memory app". Worlds cannot fix the provider's side. |
| **Not configured** | Fixture: Local Weather, never set up. | Chip "Not configured"; fixture summary; no facts, no placeholder. | "Open provider settings". |

Rule: Stale and "waiting" are separate. A reassuring state never hides a stale one, and an unknown or unclassified case is shown as Unknown, never as fine. Unavailable text is chosen by cause; one sentence is never reused for a different cause.

### 4.10 Disconnect (D1) and back to empty

`#disconnect` is always visible for the selected service and opens a confirm that **names the service and says what goes and what stays**. It is a dialog on desktop and tablet and a bottom sheet on phone. It is a consequential surface: no Dot, no flourish. **Disconnect deliberately keeps the credential reference** (the contract's `credentials_retained`); nothing is deleted from anywhere but Worlds' counter. Removing the reference is a different action with a different confirmation (R1, §4.11).

| Goes (from Worlds) | Stays |
|---|---|
| The slip, its facts, its receipts, its app frame, and the kept-out note. | The owning app, untouched. Nothing is sent to it and nothing in it changes. |
| What Worlds has remembered of the last-known values. | The credential reference (held, nothing is sent with it), listed in Settings as "Held, not in use". |

Buttons: **Disconnect** (danger) and **Keep service**. Escape = Keep service. Focus: opens on the heading; Keep service is the first button (a safe default); on close, focus returns to `#disconnect` or, after a successful disconnect, to the next slip (or to `#add-service` when none is left).

**Optimistic then settled:** the slip, facts and frame leave in the same frame the confirm is pressed; a status message then names exactly what went ("Notes Sync's 3 facts and its app frame are removed from Worlds. The Memory app is untouched. Its reference is kept.") and offers "Reconnect" (Connect with the service and reference prefilled). If it was the last service: H0 (empty counter), Dot falls asleep (200ms, none under reduced motion). **Worlds still works:** Memory and Settings show their empty states; Connect still starts.

### 4.11 Revoke (R1)

Revoke is the contract's `revoke`: it deletes the connection's config, **purges the held credential reference**, and reports `external_state_untouched: true`. It is reached from Settings > Credential references > **Remove** (also on a "Held, not in use" row). It is quiet like disconnect.

| Goes (from Worlds) | Stays |
|---|---|
| The credential reference and anything Worlds held for it. | The owning app, untouched. |
| The connection's config. If the service is still on the counter, it is disconnected as well, because it can't publish without its reference: its slip, facts, receipts and frame leave too. | **The provider's own key.** Worlds can't revoke it. The person should rotate or revoke it with {App}, the owning app. |
| The service becomes a Discover candidate again ("Not connected yet · no reading"). | Anything the owning app holds. |

Dialog: title "Remove {Name}'s reference?"; the table above in plain sentences; "Worlds can't revoke the key itself. Rotate it with the {App} app." Buttons **Remove reference** (danger) and **Keep reference**; Escape = Keep reference; focus as D1. After: "{Name}'s reference is removed and its connection is gone from Worlds. The key itself is still active with {App}. Rotate it there." plus "Open {App}". If this empties the counter: H0.

### 4.12 Memory: Agent facts (M1)

Memory is the **Guide** door: a trail, not a flat inventory. One list of every fact from every connected service, using the **same fact-row component** as S1, so one fact reads the same everywhere.

- **Service headings** carry the owner link ("Open Media app") and the service's state word and freshness, so a screen-reader user can jump by heading and a person can follow any fact to its owner.
- Each **row**: label, value (or Unknown / Unavailable), state word, source (local, external or none), "observed {time}", **Agent can** and **Human can** as two labelled lines, the fixture reason where there is one, and the send-scope word for the service's reference ("Scoped to {n} capabilities" or "Unscoped").
- **Unknown rows** add: "Worlds can't see it. {fixture unknownReason} What would help: the owning app answering again." with the owner link.
- A `none` permission renders as a named, focusable, disabled control with its reason.
- Filter: "All services" or one service (a select, keyboard reachable).
- When a change lands, affected rows settle with the same 60ms tick as the receipt (item 10). Dot rests in the page header beside the freshness line.
- **Empty:** the counter's dashed empty well with Dot asleep: "No agent facts yet. Connect a service to add facts without guessing." with `#add-service`.

**Who sees, who acts** words: a person reads "Agent can: see it" and "Human can: act on it in the Home app". Values and permissions change only by an event the owner caused (§9), never by Worlds on its own.

### 4.13 Settings (T1-T5)

Desktop and tablet: a section sub-list beside the pane. Phone: one scrolling column of section headings; the page is reached from the header and the header carries a Back path to where the person came from. Dot rests in the page header beside the live line. Sections, in this order:

1. **Connected services:** one row per service: name, state word, freshness, its own `#disconnect-{service-id}` (same confirm as D1). Empty: "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you."
2. **Credential references:** heading line "Worlds stores a credential reference, never the secret." Each row: service, the reference text (wraps, never truncated), **send scope** ("Scoped to {n} capabilities: {list}" or **"Unscoped (older connection): this reference can be used for anything this service offers"**), and Replace, Remove (opens R1) and, for unscoped ones, "Narrow scope". **"Narrow scope" opens Connect at Reference (C2) for that connection with its capabilities pre-filled and ticked**; for an older connection with no stored capability list, Connect first runs the read-only Discover for that service and then lands on C2. The welcome's references (`ref://media-shelf`, `ref://front-door`) are labelled mock handles and are **Scoped**. To show the legacy label (Sol condition 3) a reviewer uses the About mock control "Pretend Media Shelf has an older, unscoped connection". Replace is an inline field with the same checks and secret refusal. A kept reference from a disconnected service appears as "Held, not in use". Empty: "No references stored. Connecting a service will add one."
3. **Providers:** Local Weather, Not configured. Fixture summary and `emptyReason` shown; no facts; "Open provider settings" → `app://settings/providers`.
4. **Live feed:** `#settings-live` switch (the same state as `#live-toggle`; the two always agree); check cadence 5s (default), 10s, 30s; **Motion:** Follow system (default) / Reduce always; **Theme:** Dark (default), Light, High contrast, Low stimulation, Follow system; **Text size** note ("Worlds follows your browser's text size; zoom to 200% reflows"). Line: "Reduced motion removes animation, not data."
5. **About:** MOCKUP, "Synthetic data only", how Worlds fits the Field Guide (one sentence and link), the UNKNOWNs (§12), "Replay the welcome", "How one fact travels", a disabled named "Keep history after disconnect" with reason "Not decided yet; this mockup keeps nothing", and **Mock controls** for reviewers, all of which only produce *failure or edge* conditions: "Make Media Shelf stale now", "Pretend Notes Sync refused its reference", "Pretend Media Shelf has an older, unscoped connection". **There is no control here, or anywhere in the UI, that makes approvals look connected** (§10).

States per section: loading (static Dot, "Dot is fetching your settings…"), empty (as above), error ("Couldn't save that. Nothing was changed. Try again."), long references wrap.

### 4.14 Offline, error, loading (X1)

- **Offline** (the browser has no connection): a strip under the top bar, "You're offline. Worlds is showing what it last knew, marked with when." The live feed pauses (`#live-toggle` reads "Paused · offline"), ages stop growing (they would be false), and states are shown as Stale-with-time. Returns automatically: "Back online. Checking now."
- **Couldn't load a screen:** "Worlds couldn't load this. Your services and references weren't changed." "Try again" and "Go to Home". Dot rests; no blame.
- **Save failed:** "Couldn't save that. Nothing was changed. Try again."
- **Loading:** always static (a still Dot and a sentence). No spinners, no shimmer.

## 5. Layouts

Three layouts, designed separately. Breakpoints in rem so they follow text size: **desktop ≥ 60rem** (designed at 90rem, which is 1440px); **tablet 48rem to under 60rem** (designed at 48rem, 768px); **phone under 48rem** (designed at 24.375rem, 390px). DOM order equals reading order at every width; the layout changes by CSS, not by reordering content. No sideways scroll; at 200% zoom content reflows and nothing clips.

### 5.1 Desktop (1440)

- **Top bar** (sticky): MOCKUP label and "Worlds"; then `#live-toggle` ("Live · checked 24s ago" / "Paused · frozen at 24s"), `#simulate-change`, `#add-service`, `#disconnect` ("Disconnect Front Door", or disabled "Disconnect" when none).
- **Rail** (left, `--surface-sunken`): Home, Connect, Needs you (badge), Memory; Settings at the foot. Icon and label always visible.
- **Home:** two columns: slips (a column of roughly 23rem) and the detail pane. Pane order: header, fact list, receipt (hero), Agent sees, app frame.
- **Connect:** vertical step list at the left of the page, step body in the middle, a summary of what will happen at the right if width allows; Back/Next bar at the foot of the body.
- **Needs you and Memory:** single reading column up to a comfortable measure (about 55ch for prose), cards full width.
- **Settings:** a 14rem section list and the pane.
- **Dialogs:** centred, `--surface-raised`, scrim `--surface-sunken`.

### 5.2 Tablet (768)

- Top bar wraps to two rows (title and live controls, then add and disconnect). The rail is a narrow strip with **icon above a short label, always visible** (hover does not exist on touch, so labels are never hover-only); the accessible name is the label. On keyboard focus a tooltip with the full name also appears.
- **Home:** slips become a two-column grid above the detail pane; the pane is full width; receipt stations stay three across (they fit at 768) and drop to a column under 40rem.
- **Connect:** step list becomes a horizontal numbered strip above the body.
- **Settings:** the section list becomes a wrapped row of links above the pane.
- Targets stay 44px; gaps do not collapse.

### 5.3 Phone (390)

Own navigation and reading order, not a squeezed desktop.

- **Page header:** MOCKUP label, title, and a **Settings** text-and-icon button at the right (at least 44px) on every top-level screen. This is the only route to Settings on phone; on the Settings screen it becomes Back.
- **Status row:** the one-line status ("1 needs you · checked 24s ago") then `#live-toggle` and `#simulate-change` as two 44px-high buttons (wrap to two lines if needed).
- **Selected-service card:** state chip, reason, "Open Front Door" and `#disconnect` ("Disconnect Front Door") on the first screen. With none selected: the honest line plus disabled `#disconnect` with its reason.
- **Slips:** full width, each at least 44px, state word and freshness visible.
- **`#add-service`:** a floating 44px-high pill at right 1rem, bottom `calc(<tab bar height> + env(safe-area-inset-bottom) + 1rem)`; the list has enough bottom padding so it never covers a slip. Hidden on pushed screens and in Connect.
- **Bottom tab bar:** the **four doors**: Home, Connect, Needs you (with count in text), Memory. Four equal tabs, each at least 44px tall and wide (the bar is 3.5rem plus safe area). The tab bar is `aria-label="Worlds sections"`.

**Exactly one bottom strip at a time:**

| Screen | Bottom strip |
|---|---|
| Home, Needs you, Memory, Settings | Tab bar |
| Pushed service screen | none; Back is in the top bar |
| Connect | the sticky Back / Next bar (replaces the tab bar) |
| Dialogs (disconnect, revoke, handoff) | bottom sheet over the tab bar, scrim behind |
| Offline strip | top, under the page header (never a second bottom strip) |

**Pushed service screen:** Back (top bar), the status row, then `#disconnect` under the heading, then tabs **Facts / App / Agent view**; Facts first; receipt first inside Facts. Focus moves to the new heading on every push and returns to the invoking slip on Back.

## 6. Components and tokens

Tokens are the house names in `docs/vnext/house/tokens.css`. **No colour, gap, radius or text size is written as a literal outside tokens or the local handles below.** Lengths that have no token (target floor, hairlines, panel widths, durations) are Worlds-local handles with a `--wv-` prefix, defined once, in `rem`; they hold no colour. (The tokens document is explicit that the target floor is Play-Nice's and has no token; Q8 asks whether these handles should be promoted.)

**Local handles (layout, not colour):** `--wv-target-min` (the 44px floor, as 2.75rem), `--wv-hairline`, `--wv-frame-line`, `--wv-ring-width`, `--wv-rail-w`, `--wv-slips-w`, `--wv-ease` (`cubic-bezier(.2,0,0,1)`), `--wv-dur-state` (150ms; 120ms phone), `--wv-dur-stagger` (60ms), `--wv-dur-sheet` (200ms), `--wv-dur-nod` (200ms), `--wv-dur-carry` (400ms; 300ms phone). Reduced motion sets durations to 0 and travel to 0.

**Display handles (the product's typographic decision past `h1`, composed from `--type-h1` per `house/DESIGN.md`):** `--wv-display-page` (2.5rem) for page titles, service titles and the welcome heading; `--wv-display-section` (1.85rem) for section headings. Both are plain `rem`, aliasing upward from `--type-h1`; neither is fluid. They keep V2's roughly 2.5:1 display-to-body step, which is where the "printed, hand-labelled" air comes from. `--type-lead` and `--type-body` are used for everything else.

| Component | Tokens it uses | States |
|---|---|---|
| **App shell** (top bar, rail, tab bar, main) | `--surface-base` page, `--surface-raised` top bar and tab bar, `--surface-sunken` rail, `--border-line` edges, `--space-*` | wide, tablet, phone; offline strip |
| **MOCKUP label** | `--accent-mint` text and border, `--radius-sm`, `--type-micro` | always visible on every screen |
| **Status line** | `--text-soft`, `--status-needs-you` for the count word, `--type-body` | quiet, needs you, paused, offline |
| **Live toggle / Simulate** | button tokens; `aria-pressed` on the toggle | on, paused, offline; focus, hover, active, disabled |
| **Button** (primary, secondary, danger, text, icon) | primary: `--action-primary`, `--action-on-primary`; secondary: `--surface-raised`, `--border-line-strong`, `--text-primary`; danger: `--status-failed` border and text; `--radius-full`; `--type-label` | default, hover, focus-visible (`--focus-ring`), active, disabled, pressed; at least `--wv-target-min` tall |
| **State chip** | `--status-*` text on `--surface-raised` or `--surface-sunken`, a glyph, the word, `--type-small` | healthy, needs you, stale, unavailable, unknown, not configured; never glyph or colour alone |
| **Slip** | `--surface-raised`, `--radius-lg`, `--border-line`, selected `--action-primary` border and the word "Selected" | default, hover (lifts), selected, focus, with stale/unknown/unavailable content |
| **Page and service header** | `--text-primary`, Young Serif, `--wv-display-page` | per state; Dot rests beside the freshness line |
| **Section heading** | Young Serif, `--wv-display-section` | n/a |
| **Fact row** | `--surface-raised`, `--border-line`, `--type-lead` for the value | live, stale, unavailable, unknown; permission none; settling |
| **Receipt** | frame `--border-line-strong`, `--radius-lg`, `--surface-raised`; stations `--surface-sunken`, `--radius-md` | idle, updating (ticks settle), reduced-motion (all ticked at once) |
| **Agent can / Human can lines** | `--text-primary`, `--text-soft` for reasons | read, act, write, none (disabled and named) |
| **Provenance row** | `--text-soft`, `--type-small` | welcome, connected later, unscoped, scoped |
| **App frame** | `--surface-raised`, `--border-line`, inner `--surface-sunken` | loading, loaded, refused, timeout |
| **Owner retry row** | `--surface-sunken`, `--border-line` | shown with attempts and last error; a link only, no command |
| **Proposal row (sample)** | `--surface-sunken`, dashed `--border-line-strong` | proposed (inert); never active in this mock; carries a "Sample" word |
| **Handoff panel** | `--surface-raised`, `--border-line-strong` | not available (the only built state) |
| **Connect stepper** | progress uses `--action-primary` (done) and `--border-line` (to do); current step has a numeral, a word and `aria-current="step"` | pending, current, done, error |
| **Reference field** | `--surface-sunken`, `--border-line-strong`, error `--status-failed`, valid `--status-done`, each with a word and glyph | pristine, dirty, valid, refused, pending, disabled |
| **Send-scope list** (capabilities) | checkboxes on `--surface-sunken` rows | all, some, none (invalid) |
| **Test panel** | `--surface-sunken` | idle, running, passed, failed, retry n of 3, limit |
| **Fact chooser** (publish selection) | checkbox rows | all, some, last-one-locked (`aria-disabled`, with its reason) |
| **Dot** | shell `--accent-apricot`, body `--accent-mint`, ink `--surface-base`, parcel paper `--surface-raised` with `--border-line-strong` | asleep, awake, nod, parcel, resting; `aria-hidden` except where a sentence names Dot |
| **Stamp** | `--status-done` border and text, Young Serif capitals, tilted -4° | landing (200ms), static (reduced motion: no travel) |
| **Dialog / sheet** (disconnect, revoke, handoff) | panel `--surface-raised`, scrim `--surface-sunken`, `--radius-lg` | open, working, closed; Escape closes |
| **Toast / live line** | `--surface-raised`, `--status-done` border for success, one `role="status"` | hidden, visible (3.6s, persistent while paused) |
| **Empty state** | dashed `--border-line-strong`, `--text-soft` | first run, after disconnect, empty list (also Memory) |
| **Settings section, switch, select, radios** | `--surface-raised`, `--border-line-strong` | each with loading, empty, error |
| **Banner** (offline, error) | `--surface-raised`, `--status-stale` or `--status-failed` edge with a word | offline, back online, load error |

**Type.** Display: Young Serif (400); body: Atkinson Hyperlegible Next (400, 600 for labels, 700 for state words); system fallbacks (Georgia, Verdana) and **no external fonts in the prototype**. Roles: page and service titles `--wv-display-page`; section headings `--wv-display-section`; slip and card titles `--type-lead`; body `--type-body`; labels and chips `--type-label`; captions `--type-small`; MOCKUP label `--type-micro`.

**Icons.** Status glyphs are a single simple line set (check, alert triangle, clock, x in circle, question mark, dash), always beside their word. The house painted sticker icons may be used for nav in production at 20px or more beside their label; the prototype uses plain inline line glyphs as stand-ins. No emoji, no gradient.

## 7. Copy

Rule: machine truth, then plain words, then (sometimes) a warm line. Warmth never creates a state, a deadline or certainty. Short sentences; say what happened, what it means, and what to do. Avoid "provider", "capability", "endpoint" and "scope" in headings; gloss them when they must appear.

### 7.1 Status words and short labels
Healthy · Needs you · Stale · Unavailable · Unknown · Not configured · Working · Done · Failed. Freshness: "Checked 24s ago", "3m ago", "2h ago", "Never checked". Source: "From this device", "From the service's provider", "No source answered". Permissions: **Agent can:** see it · act on it · edit the local copy · not at all. **Human can:** see it · act on it in the {app} · edit the local copy · not at all.

### 7.2 Strings by screen

| Where | String |
|---|---|
| Top bar | "MOCKUP" · "Live · checked 24s ago" · "Paused · frozen at 24s" · "Paused · offline" · "Simulate change" (full name for assistive tech: "Simulate the Home app reporting the back door locked") · "Add service" · "Disconnect Front Door" · disabled: "Disconnect" with "Connect a service before disconnecting" |
| W1 | "Hello, I'm Dot." / "Worlds is a small counter. Your services leave their facts here, and Worlds keeps them honest." / "Connect a service" / "Skip welcome" |
| W2 | "First, Media Shelf." / "Worlds will send this reference only when reading: {list}." / "Passed. Nothing on Media Shelf was changed." / "Connect Media Shelf" / "Skip welcome" |
| W3 | "And one more: Front Door." / "Connect Front Door" / "Skip welcome" |
| First-receipt note | "Here's one fact on its way." / "Media Shelf says it holds 1,284 items. Worlds shows where that came from and who can see it or act on it." / "Every fact says where it came from and who can act on it." |
| H0 | "The counter is quiet." / "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you." / "No services are connected yet." |
| H1 status | "1 needs you · checked 24s ago" · "Nothing needs you right now." · "That was the last one. Nothing needs you right now." |
| Receipt | "Source" · "Worlds" · "Agent / Human" · "Shared as {label}." · "Connected during first run · reference held, never the secret." · "Connected {time} · reference held, never the secret." · "Scoped to {n} capabilities" · "Unscoped" · "Updated just now" |
| Kept out | "{n} fact is kept out of Worlds. You chose not to share it." |
| Stale | fixture summary, then "Nothing has changed since {time} that Worlds knows of." · button "Check now" |
| Unknown | "Worlds has no current reading for this and won't guess." |
| Unavailable (provider silent) | "That service stopped answering. Its last facts are still here, marked with when Worlds checked." |
| Unavailable (job rejected) | "The printer answered, so Worlds is not treating it as silent." |
| Reference refused | "{Name} stopped accepting this reference. Its last facts are still here, marked with when Worlds checked. Worlds can't fix this on {Name}'s side." · "Replace reference" |
| Not configured | "{Name} isn't set up. Open its settings." |
| Frame | "This app didn't load inside Worlds." · "Use the link below, or try again." · "Try again" · "Open Home app" · "This app doesn't receive Worlds' credentials." |
| Needs you | "Needs you" · "A decision is required; the local app will not act on its own." · "Open Home app" · "Ask Project Home to review" · "Not available yet" · "Nothing needs you right now." |
| Owner retry | "{Label}, tried {n} time(s). Last error: {lastError}." / "This happens in {App}, by you. Worlds can't retry it." |
| Handoff | "Approvals aren't connected yet." · "Worlds can't send this to Project Home yet, so nothing was sent. You can decide in the Home app instead." · "When approvals are connected, this will open Project Home with the proposal. Project Home, not Worlds, makes the decision." |
| Proposal sample | "Sample. The fixture has no proposals." · "Proposed. Not active." · "Worlds can't run this. It can only show it." · "Decided by: not known yet." · "Agent can't do this." |
| Simulate | "The Home app reported the back door locked. Worlds locked nothing." |
| C1 | "Find a service" / "Looking only. No reference has been used yet. This sends no credential and changes nothing." / "Not connected yet · no reading" / "not verified yet" / "Auth: unknown" / "Already at the counter" / "No other services to connect right now." |
| C2 | "Credential reference" / "A reference is a name that points to a secret kept somewhere safe. Worlds stores the name, never the secret." / "Worlds will send this reference only when reading: {list}." / "This is about what the reference can be used for. It is not what appears on your counter." / pristine "Not edited yet. Use the form ref://name." / dirty "Edited. Worlds checks it when you leave the field." / valid "Looks like a reference." |
| C2 refused | "That looks like a secret, not a reference. Nothing was saved and the field was cleared. Worlds stores a credential reference, never the secret. Use something like ref://notes-sync." |
| C2 own secret | "That reference points at one of Worlds' own secrets, so it can't be used for a service." |
| C3 | "Run safe test" / "Testing…" / "Passed. Worlds read {n} facts in {ms}ms. Nothing on {Name} was changed." / failed "That didn't go through, and the reference is still here. Try again whenever you like." / cause "{lastError}" / "last tried {n}s ago" / "try {n} of 3" / limit "That's three tries. Check the reference with the owning app, then edit it to start again." / mock "Simulate a failed test (mock)" |
| C4 | loading "Dot is fetching the facts…" / empty "This service offers no facts yet. Nothing is guessed." / "These are separate facts and stay separate." |
| C5 | "Choose what to show" / "Shown on your counter and visible to agents: {list}." / "At least one fact must stay shared." |
| C6 | "Publish" / "Dot is carrying the parcel to the counter…" / "Connected" / "{Name} is at the counter." / "Open {Name}" / "Add one more" / "Manage this reference in Settings" |
| D1 | title "Disconnect {Name}?" / "{Name}'s {n} facts and its app frame will be removed from Worlds. The {App} app is untouched." / "Its reference stays in Settings, held and unused." / "Disconnect" / "Keep service" / after: "{Name}'s {n} facts and its app frame are removed from Worlds. The {App} app is untouched. Its reference is kept." · "Reconnect" |
| R1 | title "Remove {Name}'s reference?" / "Worlds will delete the reference and everything it held for it. {Name} is disconnected too if it is still at the counter. {App} is untouched." / "Worlds can't revoke the key itself. Rotate it with the {App} app." / "Remove reference" / "Keep reference" / after: "{Name}'s reference is removed and its connection is gone from Worlds. The key itself is still active with {App}. Rotate it there." · "Open {App}" |
| Settings | "Worlds stores a credential reference, never the secret." / "No references stored. Connecting a service will add one." / "Unscoped (older connection): this reference can be used for anything this service offers." / "Narrow scope" / "Reduced motion removes animation, not data." / "Not decided yet; this mockup keeps nothing." / "Couldn't save that. Nothing was changed. Try again." |
| Memory | "No agent facts yet. Connect a service to add facts without guessing." / "Worlds can't see it. {unknownReason} What would help: the owning app answering again." |
| Offline | "You're offline. Worlds is showing what it last knew, marked with when." / "Back online. Checking now." / "Worlds couldn't load this. Your services and references weren't changed." |

### 7.3 Announcements (one polite live region)
Routine ticks are **not** announced. Batched, polite messages only for: published ("Connected Notes Sync. Its slip and facts are now at the counter."), disconnected (the D1 sentence), reference removed (the R1 sentence), a state change ("Media Shelf is now Stale."), a failed test (once, assertive: "Test didn't pass."), live feed paused or resumed, and "Updated across Home, Front Door and Agent facts."

## 8. Motion and accessibility

### 8.1 Motion

One curve, `--wv-ease`. Motion confirms a change that has already happened.

| What moves | Why | Normal | Reduced motion |
|---|---|---|---|
| State word on a slip, chip and receipt | confirm a change | cross-fade 150ms (120ms phone) | instant swap, opacity only |
| Receipt stations and Memory rows | show the journey settling | tick at Source, Worlds, Agent / Human, 60ms apart | all ticked at once, no stagger |
| Dot nod | "I noticed" beside "Updated just now" | 200ms, once | no nod; "Updated just now" stays 3s |
| Dot's parcel | publishing, welcome connections | travels 400ms desktop, 300ms phone | parcel appears at the counter, no travel |
| Connected stamp | the proud moment | scales in and lands tilted, 200ms | appears in place |
| Slip hover and select | affordance | lifts about 2px; scale only on buttons, 1.03, off on touch | no transform |
| Dialog and sheet | context | 200ms | none |
| Step change in Connect | continuity | content fades 150ms | instant |
| Plain feed tick | freshness | only the age number changes | same |

Respect the OS reduced-motion setting unconditionally; Settings can additionally force "Reduce always". Reduced motion removes animation, never data. Nothing flashes. No parallax. Dot's idle pose is still.

### 8.2 Keyboard map
- **No single-key shortcuts** (avoids accidental triggers and conflicts). Everything is reached with Tab, Shift+Tab, Enter, Space, arrow keys inside radio groups and selects, and Escape.
- **Skip link** "Skip to Worlds" is first; it moves focus to `main`.
- **Tab order** equals reading order: skip link; top bar controls (`#live-toggle`, `#simulate-change`, `#add-service`, `#disconnect`); nav (rail or tab bar; on phone the header Settings button comes before main); main content in the order in §4; dialogs trap focus and return it.
- **Slips:** each is a button with `aria-pressed`; Enter or Space selects. **Fact rows:** buttons with `aria-pressed`. **Step list:** not focusable except the Back / Next buttons.
- **Escape** closes dialogs, sheets and the handoff panel and returns focus to the invoking control. Escape on Connect asks nothing when nothing is saved.
- Disabled-with-reason controls use `aria-disabled="true"` so they stay focusable and readable, never the `disabled` attribute that drops them from the tab order (permission `none`, disabled `#disconnect`, the last chosen fact, the history option, the test after three tries).
- **Focus visible** everywhere: `--focus-ring`, thick, offset, from tokens in every theme. Focus is never hidden behind the top bar or the tab bar (scroll margin set).
- **Focus moves** to the new heading on every push, step change and destination change (`tabindex="-1"`), and returns to the invoking control on Back or close.

### 8.3 Screen-reader structure
- Landmarks: `header` (banner), `nav` ("Worlds sections"), `main`, and a single `role="status"` live region.
- One `h1` per screen (the screen's name); `h2` per section; slips list is a `ul` of buttons labelled with name and state ("Front Door, Needs you, checked 24s ago, selected"); the receipt is a section with `h3` per station.
- State is always text. Dot is `aria-hidden` unless a sentence names it; the stamp reads "Connected".
- The step list is an `ol` with `aria-current="step"`; a phone "Step 3 of 6 · Test" line is real text.
- Forms: visible labels, errors tied with `aria-describedby`, `aria-invalid`, and the refusal never repeats the secret.
- Tables are not used for layout; Memory rows are articles under service headings.

### 8.4 Targets, reflow, text
Every interactive target is at least 44px (`--wv-target-min`) with 8px (`--space-sm`) gaps; nothing under `--type-micro` except the MOCKUP label; long values wrap (`overflow-wrap`), never truncate; browser zoom and text scaling are never blocked, and at 200% the page reflows without sideways scroll.

### 8.5 Contrast in all four house modes
Measured from `tokens.css` (WCAG ratios; text needs 4.5:1, non-text and borders 3:1). `base` = `--surface-base`, `raised` = `--surface-raised`, `sunken` = `--surface-sunken`.

| Pair | Dark | Light | High contrast | Low stimulation |
|---|---|---|---|---|
| `--text-primary` on base / raised / sunken | 13.8 / 12.1 / 15.1 | 14.4 / 15.5 / 12.6 | 18.2 / 15.5 / 19.1 | 12.8 / 11.8 / 14.1 |
| `--text-soft` on base / raised / sunken | 10.2 / 8.9 / 11.2 | 7.2 / 7.8 / 6.3 | 16.2 / 13.8 / 17.0 | 9.0 / 8.3 / 9.9 |
| `--action-on-primary` on `--action-primary` | 11.3 | 7.6 | 15.6 | 8.7 |
| `--action-primary` on raised (as link or border) | 9.9 | 7.6 | 13.3 | 7.3 |
| `--focus-ring` on base / raised / sunken | 13.3 / 11.7 / 14.6 | 14.4 / 15.5 / 12.6 | 18.2 / 15.5 / 19.1 | 11.3 / 10.4 / 12.4 |
| `--border-line` on base / raised | 3.9 / 3.4 | 3.7 / 4.0 | 18.2 / 15.5 | 3.9 / 3.6 |
| `--border-line-strong` on raised / sunken | 5.4 / 6.8 | 7.8 / 6.3 | 15.5 / 19.1 | 5.1 / 6.1 |
| `--status-healthy` on raised / sunken | 10.1 / 12.7 | 6.1 / 4.95 | 13.4 / 16.4 | 8.9 / 10.6 |
| `--status-needs_attention` and `--status-stale` on raised / sunken | 9.5 / 11.9 | 6.6 / 5.4 | 12.0 / 14.8 | 8.6 / 10.2 (needs-you) · 7.3 / 8.6 (stale) |
| `--status-unavailable` on raised / sunken | 7.7 / 9.6 | 6.9 / 5.6 | 10.5 / 13.0 | 7.2 / 8.5 |
| `--status-unknown` and `--status-not_configured` on raised / sunken | 7.7 / 9.7 | 7.8 / 6.3 | 13.8 / 17.0 | 7.3 / 8.6 |

Every pair passes. Two things the numbers cannot do, so the design handles them in words and shapes: (1) in Dark, Light and High contrast "Needs you" and "Stale" share one colour, and in Low stimulation "Stale", "Unknown" and "Not configured" share one; the word and the glyph are what tell them apart. (2) In Light the Dot's apricot shell and mint body are dark tones, so Dot's ink (`--surface-base`) is light; it must be checked once rendered. The `--border-line` values pass 3:1 but are the lowest ratios in the system; interactive controls therefore use `--border-line-strong`. These are token measurements, not a check of the built prototype (Q9).

## 9. Reactivity

**One in-memory store keyed by fact id** feeds the slips, the service surface, the receipt, Memory, Settings, the attention count and the top bar. Every change re-renders all of them in one pass. Ages are always recomputed from `checkedAt`, never taken from the fixture's `ageSeconds`.

- **Mock clock** starts at the fixture's `generatedAt` (2026-10-13T14:24:26Z) and runs in real time, so first paint reads Front Door "checked 24s ago" and Media Shelf "checked 28s ago". No invented numbers.
- **Tick:** every 5s by default (Settings: 10s or 30s). A tick updates every age everywhere and nothing else moves.
- **Re-check rule (per fact, not per service).** Any fact whose state is Live re-checks every 30s while the feed is on. A re-check changes `checkedAt` and `observedAt`, never the value. Facts that are Needs you, Unavailable, Unknown, Not configured or already Stale never re-check; their ages grow and their words stay. A fourth service needs no new line: the rule applies to whatever is connected.
- **Scripted missed re-check (stale event), the proof path.** Each session arms **two** scripted misses, both labelled "simulated": one on Media Shelf's first live fact, and one on the **first live fact published through Connect** (for Notes Sync, "Local notes waiting to upload"). 90s after the fact becomes live (the welcome counts as first load for Media Shelf; Publish for the Connect one), it misses a re-check and becomes **Stale (simulated)**, keeping its last value as a labelled "last known". "Check now" restores it to Live. If it is left, **60s later** it becomes **Unknown (simulated)**, still showing the last value only as "last known". Pausing freezes both timers. Each fact is scripted once per session, and a service's own state word keeps following its fixture state; the fact carries its own state (facts are never merged).
- **`#simulate-change`:** the Home app reports the back door locked (fixture `crossViewExample`, `lock-back-door`). In one frame: `frontdoor.back-door` reads "locked", Front Door becomes Healthy, the attention count goes 1 → 0 in the status line, the Needs you badge, the slip, the service header and Memory. The change comes **from the owning app**, not from Worlds: "The Home app reported the back door locked. Worlds locked nothing." stays visible with "Open Home app". Worlds never shows `lock-back-door` as a pending proposal.
- **Optimistic then settled:** Publish, Disconnect and Revoke show their result in the same frame (the slip appears or disappears), then settle visibly (parcel and stamp; the removal message) and name what changed. A failed Publish removes the optimistic slip and says why.
- **Pause and resume:** `#live-toggle` (`aria-pressed`, on by default) freezes the clock, ticks, re-checks and the stale event; data, actions and `#simulate-change` still work. Resume continues from the frozen time. It reads "Live · checked 24s ago" or "Paused · frozen at 24s". `#settings-live` is the same state and always agrees.
- **Announcements:** routine ticks are silent; meaningful changes are batched into one polite message (§7.3).
- **Words for the feed:** every feed-driven change carries the word "simulated" where it appears, so nothing reads as a real reading.

## 10. Trust and boundaries

Sol's six conditions are pre-prototype gates (Project Home decision 145: "Approve the boundaries; Sol's six conditions become pre-prototype gates"). This design makes each visible. Items marked *gate check* are things the prototype must show before it is shown to anyone as proof.

| # | Condition | Where it appears in the UI | Gate check |
|---|---|---|---|
| 1 | **Project Home approval is fail-closed.** Never shown as working; an approval is a link or handoff that can say "not available yet". | §4.7 N1 and N2: "Ask Project Home to review" is labelled "Not available yet"; the panel says "Approvals aren't connected yet" and "nothing was sent". No "sent", "pending", "approved", spinner or recorded-handoff receipt anywhere, and no pending-approval line in any dialog (none can exist). Unknown connectivity renders as not available. **There is no Settings row, mock control or other UI path to a "connected" state.** A reviewer-only flag, if ever needed, is a build-time constant in the prototype file, off in the shipped mock and unreachable from the UI. | Click the handoff in the default build and confirm nothing reads as done; search Settings and About for any control touching approvals (there must be none). |
| 2 | **Proposed actions are inert.** Shown as proposals; never executable from Worlds. | §4.8: the one sample proposal reads "Proposed. Not active." with "Agent can't do this." There is no Run, Do it or Approve control on a proposal, and the fixture-true owner retries are links to the owning app, not buttons. `#simulate-change` models an *owner-originated* change, not execution. | Search every screen for a control that executes a proposal or a retry; there must be none. |
| 3 | **Scoped auth before any network call; legacy unscoped labelled "unscoped".** | §4.4 C2: the **send-scope** list (capabilities) and its sentence "Worlds will send this reference only when reading: {list}" must be satisfied before Test can run; Discover says no reference has been used. C5's publish selection is a separate, differently named list. Settings > Credential references and the receipt's Worlds station show "Scoped to {n} capabilities" or "Unscoped" with "Narrow scope" (which opens Connect at Reference) for unscoped ones. The welcome's connections are scoped. | Test cannot run with no scope; the C2 and C5 lists are visibly different; Unscoped is visible in Settings, Memory and the receipt after the About mock control. |
| 4-5 | **Contract fixes** (completed in the Connect contract, draft PR #329). | Not a UI element. The UI follows the contract as it stands: six verbs, `disconnect` keeps the reference (`credentials_retained`) and `revoke` is the only path that deletes it (R1), `external_state_untouched` is said plainly. | The spec and prototype cite only fields the contract has. |
| 6 | **The read-only path is proven first: keyboard, mobile, stale and unknown.** | Everything in this design before Needs you is read-only. The acceptance path (below) is run first, on a service the person actually connected. | See "Read-only path acceptance". |

**Read-only path acceptance (condition 6), the proof path.** All of this must pass before proposals, approval or any later work is built on top of it:
1. Empty counter → welcome (skip works, and so does connecting two) → Discover (no readings shown) → Reference (a secret refused and cleared; a capability scope chosen) → Test (a failure, then a pass) → Preview → Choose → Publish → Connected.
2. The **published** service's live fact goes **Stale** (word, last-known value, time), then **Unknown** (words, no number), by the scripted rule in §9 with no reviewer waiting beyond the 90s and 60s timers (or the About control "Make Media Shelf stale now" for the seeded service).
3. Follow a link out to the owning app (Open Memory app).
4. **Revoke** (what goes, what stays, the provider's key still active), and **Disconnect** (what goes, what stays, the reference kept), then the counter is empty and **Worlds still works** (Memory, Settings, Connect).
5. All of it by **keyboard alone** (visible focus, focus returns, Escape closes), and on a **390px phone** (no sideways scroll, 44px targets, one bottom strip).
6. In all four themes, and with reduced motion on.

**Other boundaries kept visible**
- **Worlds owns meaning; providers own mechanics.** Worlds shows and links; it never mints authority. A governed action goes to the owning system (Project Home for approvals, the service's own app otherwise). "Worlds locked nothing." is said in words.
- **No secret ever.** Only references are accepted, and a refused secret is cleared and never echoed.
- **Disconnect and revoke are honest and different.** Disconnect keeps the reference. Revoke deletes it and says Worlds can't revoke the service's own key.
- **Nothing is shown as a real reading.** MOCKUP label on every screen; "simulated" on every feed-driven change; synthetic data only; Discover shows no reading before one exists.
- **The frame gets no Worlds credentials.**

## 11. What the prototype must show (pointer to deliverable 2)

The prototype implements this spec as one self-contained HTML file with no network, using the house tokens only. Checker ids: `#skip-welcome` (every welcome beat), `#add-service`, `#disconnect` (visible on the first screen; confirm button labelled "Disconnect"), `#simulate-change`, `#live-toggle`, plus `#settings-live` and `#disconnect-{service-id}` in Settings. Visible MOCKUP label. Desktop, tablet and phone layouts. It must demonstrate the proof path in §10 and the failure path (a failed test, a refused secret, a refused frame, a fail-closed handoff). The shipped mock has **no UI path** to a connected approval state.

## 12. Open questions and UNKNOWNs

**Questions for Rylee or the team**
- **Q1.** Does Connect truly belong at the Explore door? The Connect contract is Platform/Capability; only its screen is Experience. If Explore later means Hive Works exploration, Connect could instead be a Home action with no door of its own.
- **Q3.** Disconnect keeps the reference (as the contract does) and revoke deletes it. Confirm the two-step shape.
- **Q5.** Status label: this design uses LANGUAGE.md's "Needs you" where V2 showed the fixture's "Needs a human". Confirm.
- **Q7.** "Human can" and "Agent can" are kept from V2; plain-word alternatives ("You can") were rejected earlier. Confirm.
- **Q8.** Should the `--wv-*` handles (target floor, hairlines, durations, rail width, the two display sizes) be promoted into house tokens, given the house rule that structural roles live in tokens and the target floor lives with Play-Nice?
- (Q2, how many services the first run offers, is settled in this spec: the welcome connects two, one at a time, and skip connects none.)

**UNKNOWN**
- **Q4.** The real sandbox rules and browser support for embedded apps.
- **Production Connect API** and credential-reference grammar. The contract's references look like `env:`, `vault:` or `file:`; the prototype uses `ref://name` from V2. These must be aligned before a real build.
- **Authority.** The fixture has no authority field, so whether Front Door or any service is governed by Project Home, and so whether "Ask Project Home to review" is ever the right route, is UNKNOWN. The sample proposal shows "Decided by: not known yet".
- **Project Home approval verification.** The contract notes the verify step has no implementation, so the path is fail-closed end to end. This design assumes "not available yet" until proven otherwise.
- **Q6.** What makes a service Stale in production (probably each read's time-to-live) versus the scripted event used here.
- **Live feed transport and cadence** in production.
- **Disconnect history policy:** whether anything is kept. The disabled "Keep history after disconnect" stays until decided.
- **Whether a fixture's facts map one-to-one to the contract's capabilities** (the send-scope list in C2 assumes a capability list per service; the prototype derives it from the fixture's facts for the proof-path services).
- **Q9.** Contrast, focus, reflow, screen-reader order and the checker are **not verified** by this document. Contrast numbers come from the tokens; they must be re-measured on the built prototype at desktop, tablet and phone sizes, in all four themes.
- **Dot in Light mode** (see §8.5) and the exact shell and body colours.

## Appendix A: Critique fixes

| Gap | Fix | Where |
|---|---|---|
| **1. Type rule flattened the display voice** | Accepted. Change #6 now says what is true: the house leaves headings past `h1` to the product, composed from its tokens; V2's *source* was loose, its design asked for a composed handle. Added `--wv-display-page` (2.5rem) and `--wv-display-section` (1.85rem) as named local handles; page, service and welcome titles use them. No `clamp()`. | §1 item 4, §2 change 6, §6 |
| **2. Welcome did not earn the counter; unearned provenance** | Took the "earn the two services" route. W1 is V2's single broad card with a sleeping Dot and one button; W2 and W3 connect Media Shelf and Front Door with the parcel and stamp each time; skip connects none and the counter is then empty. "Connected during first run" is now true of something shown. The fact-journey became a dismissible note after the first connection. Q2 is removed. | §2 change 9, §4.1, §4.3 |
| **3. Feeling specified for one screen, audited for none; phone tabs; Memory; tablet labels** | Added a per-screen Carries table (§4.0) and gave Memory, Settings and Connect a named gesture (Dot resting in the header, the 60ms settle on Memory rows, the dashed empty well). Phone bar is four doors; Settings is a header button. Memory is a trail with owner links and "what would help" on Unknown rows. Tablet rail labels are always visible, with a keyboard tooltip. | §3, §4.0, §4.12, §5.2, §5.3 |
| **4. Inert proposals built on invented fixture data** | Removed the fixture-labelled proposals. `probe-provider` and `retry-last-job` are shown as what they are (owner retries by a person, link only). `lock-back-door` appears only in its applied form. One proposal sample is drawn from the contract's `ActionProposal` shape, marked "Sample. The fixture has no proposals.", with "Decided by: not known yet" bound to `authority` (UNKNOWN). | §4.7, §4.8, §10 |
| **5. Proof path could not be walked** | Re-checks are a per-fact rule, not a table of constants. Each session arms a scripted missed re-check on Media Shelf's first live fact and on the first live fact published through Connect, with the 60s escalation to Unknown and "Check now". Discover now shows "Not connected yet · no reading", unverified capabilities and "Auth: unknown", never a state word. Acceptance step 2 runs on the published service. Because Notes Sync starts Unavailable, the fact (not the service word) carries Stale and Unknown, consistent with "facts are never merged". | §4.4, §9, §10 |
| **6. Scope screen was the publish selector** | Named two fields once: C2 is the credential's **send scope** over capabilities (contract `CredentialReference.scope`); C5 is the **publish selection** over facts. Different sentences, different words. Settings and the receipt say "capabilities". "Narrow scope" opens Connect at Reference, pre-filled (Discover first for an older connection with no stored list). | §4.4, §4.13, §10 |
| **7. A toggle could switch fail-closed approvals to connected** | Removed the Settings mock control. "Link ready" is a design note only; no UI path reaches it; any reviewer flag is a build-time constant, off and unreachable. The remaining mock controls only produce failure or edge states. | §4.7, §4.13, §10, §11 |
| **8a. Studio Printer wrong cause** | One Unavailable line per cause; Studio Printer uses the fixture's own sentence ("reachable but rejected the job"). | §4.9 |
| **8b. Stale line erased Nightly Backup's reason** | The fixture summary is always shown; the "Nothing has changed since…" line goes under it, and stands alone only where no fixture reason exists (scripted staleness). | §4.3, §4.9 |
| **8c. Unrenderable pending-approval line** | Deleted from the disconnect dialog; the gate says no pending-approval line exists in any dialog. | §4.10, §10 |
| **8d. Revoke had no screen** | Added R1 with its own three-part consequence list (what goes, what stays, the provider's key still active). Disconnect keeps the reference (`credentials_retained`) and the "Also remove the reference" tick is gone; revoke is the only path that deletes it. Acceptance step 4 walks both. | §2 change 4, §4.10, §4.11, §7.2 |

## Appendix B: decisions in force (quoted)

- 2026-10-13 Rylee: "Yeah. V2 is the only one that passed. Can we now feed that back in with all the field guide info and everything to generate a full design?"
- 2026-10-13 Rylee: "comprehensive but also keep the original feeling." Clarified: protect the feel, the look, the whimsy and the love, NOT the copy; copy may be rewritten for clarity (LANGUAGE.md).
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee, Project Home decision 145: "Approve the boundaries; Sol's six conditions become pre-prototype gates" (the Connect contract, draft PR #329).
- 2026-10-12 Rylee: "Field Guide as base" (the World Tree Field Guide is the base of the shared design system; Mulberry & Mint).

None of these is contradicted by this document. Where it departs from V2's wording (§2), it does so inside the "copy may be rewritten" clarification or to honour decision 145.


## V2 index.html (visual starting point)

```html
<!doctype html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Worlds - Welcome-led Counter V2</title>
  <style>
    :root {
      --space-xs:.25rem;--space-sm:.5rem;--space-md:1rem;--space-lg:1.5rem;--space-xl:2rem;--space-2xl:3rem;--space-3xl:4rem;
      --radius-sm:.25rem;--radius-md:.5rem;--radius-lg:1rem;--radius-full:999rem;
      --type-micro:.75rem;--type-small:.875rem;--type-label:.875rem;--type-body:1rem;--type-lead:1.25rem;--type-h1:1.5rem;
    }
    :root,:root[data-theme="dark"]{
      --surface-base:#291d30;--surface-raised:#352640;--surface-sunken:#1f1625;--text-primary:#f5ecf2;--text-soft:#dacad9;
      --border-line:#8b7791;--border-line-strong:#b09ab3;--action-primary:#a7e6ce;--action-on-primary:#291d30;--focus-ring:#c4f5e2;
      --status-needs-you:#f6cea2;--status-working:#a9d4ff;--status-held:#dcc2f7;--status-done:#c3e79b;--status-failed:#ffa8b4;--status-unknown:#cbbcce;
      --accent-mint:#a7e6ce;--accent-apricot:#f6cea2;--accent-lilac:#dcc2f7;--accent-honey:#e8c56a;
      --status-healthy:#c3e79b;--status-warning:#f6cea2;--status-needs_attention:#f6cea2;--status-degraded:#dcc2f7;--status-unavailable:#ffa8b4;
      --status-not_configured:#cbbcce;--status-disabled:#cbbcce;--status-stale:#f6cea2;--status-waiting:#dcc2f7;--status-blocked:#ffa8b4;
      --status-deferred:#dcc2f7;--status-partial:#c8ccff;--status-complete:#c3e79b;--accent-tide:#a9d4ff;--accent-kelp:#c3e79b;
      --accent-mermaid:#dcc2f7;--accent-iris:#c8ccff;--accent-coral:#ffa8b4;--accent-sunbeam:#e8c56a;color-scheme:dark;
    }
    @media(prefers-color-scheme:light){:root:not([data-theme]){--surface-base:#f7f1f4;--surface-raised:#fffafc;--surface-sunken:#ece1e8;--text-primary:#291d30;--text-soft:#5c4a62;--border-line:#8b7791;--border-line-strong:#5c4a62;--action-primary:#1f5c47;--action-on-primary:#fffafc;--focus-ring:#291d30;--status-needs-you:#8a4a12;--status-working:#2d5a8a;--status-held:#6a4a8f;--status-done:#3f6b1f;--status-failed:#9b3346;--status-unknown:#5c4a62;--accent-mint:#1f5c47;--accent-apricot:#8a4a12;--accent-lilac:#6a4a8f;--accent-honey:#7a5208;--status-healthy:#3f6b1f;--status-warning:#8a4a12;--status-needs_attention:#8a4a12;--status-degraded:#6a4a8f;--status-unavailable:#9b3346;--status-not_configured:#5c4a62;--status-disabled:#5c4a62;--status-stale:#8a4a12;--status-waiting:#6a4a8f;--status-blocked:#9b3346;--status-deferred:#6a4a8f;--status-partial:#4b4f9b;--status-complete:#3f6b1f;--accent-tide:#2d5a8a;--accent-kelp:#3f6b1f;--accent-mermaid:#6a4a8f;--accent-iris:#4b4f9b;--accent-coral:#9b3346;--accent-sunbeam:#7a5208;color-scheme:light}}
    :root[data-theme="light"]{--surface-base:#f7f1f4;--surface-raised:#fffafc;--surface-sunken:#ece1e8;--text-primary:#291d30;--text-soft:#5c4a62;--border-line:#8b7791;--border-line-strong:#5c4a62;--action-primary:#1f5c47;--action-on-primary:#fffafc;--focus-ring:#291d30;--status-needs-you:#8a4a12;--status-working:#2d5a8a;--status-held:#6a4a8f;--status-done:#3f6b1f;--status-failed:#9b3346;--status-unknown:#5c4a62;--accent-mint:#1f5c47;--accent-apricot:#8a4a12;--accent-lilac:#6a4a8f;--accent-honey:#7a5208;--status-healthy:#3f6b1f;--status-warning:#8a4a12;--status-needs_attention:#8a4a12;--status-degraded:#6a4a8f;--status-unavailable:#9b3346;--status-not_configured:#5c4a62;--status-disabled:#5c4a62;--status-stale:#8a4a12;--status-waiting:#6a4a8f;--status-blocked:#9b3346;--status-deferred:#6a4a8f;--status-partial:#4b4f9b;--status-complete:#3f6b1f;--accent-tide:#2d5a8a;--accent-kelp:#3f6b1f;--accent-mermaid:#6a4a8f;--accent-iris:#4b4f9b;--accent-coral:#9b3346;--accent-sunbeam:#7a5208;color-scheme:light}
    :root[data-theme="high-contrast"]{--surface-base:#170f1b;--surface-raised:#291d30;--surface-sunken:#0f0912;--text-primary:#fffafd;--text-soft:#f5ecf2;--border-line:#fffafd;--border-line-strong:#fffafd;--action-primary:#c4f5e2;--action-on-primary:#170f1b;--focus-ring:#fffafd;--status-needs-you:#ffd9b0;--status-working:#c2e0ff;--status-held:#e8d6ff;--status-done:#d6f5b0;--status-failed:#ffc2cb;--status-unknown:#f5ecf2;--accent-mint:#c4f5e2;--accent-apricot:#ffd9b0;--accent-lilac:#e8d6ff;--accent-honey:#f0d27c;--status-healthy:#d6f5b0;--status-warning:#ffd9b0;--status-needs_attention:#ffd9b0;--status-degraded:#e8d6ff;--status-unavailable:#ffc2cb;--status-not_configured:#f5ecf2;--status-disabled:#f5ecf2;--status-stale:#ffd9b0;--status-waiting:#e8d6ff;--status-blocked:#ffc2cb;--status-deferred:#e8d6ff;--status-partial:#d8dbff;--status-complete:#d6f5b0;--accent-tide:#c2e0ff;--accent-kelp:#d6f5b0;--accent-mermaid:#e8d6ff;--accent-iris:#d8dbff;--accent-coral:#ffc2cb;--accent-sunbeam:#f0d27c;color-scheme:dark}
    :root[data-theme="low-stimulation"]{--surface-base:#291d30;--surface-raised:#30233a;--surface-sunken:#1f1625;--text-primary:#ece4ea;--text-soft:#cbbfca;--border-line:#8b7791;--border-line-strong:#a593a8;--action-primary:#7fc4aa;--action-on-primary:#1f1625;--focus-ring:#a7e6ce;--status-needs-you:#e3c09a;--status-working:#9cc3e8;--status-held:#c9b4e0;--status-done:#b3d394;--status-failed:#e9a2ac;--status-unknown:#bfb2c2;--accent-mint:#9bd3bd;--accent-apricot:#e3c09a;--accent-lilac:#c9b4e0;--accent-honey:#d4b56a;--status-healthy:#b3d394;--status-warning:#e3c09a;--status-needs_attention:#e3c09a;--status-degraded:#c9b4e0;--status-unavailable:#e9a2ac;--status-not_configured:#bfb2c2;--status-disabled:#bfb2c2;--status-stale:#bfb2c2;--status-waiting:#c9b4e0;--status-blocked:#e9a2ac;--status-deferred:#c9b4e0;--status-partial:#b8bbe8;--status-complete:#b3d394;--accent-tide:#9cc3e8;--accent-kelp:#b3d394;--accent-mermaid:#c9b4e0;--accent-iris:#b8bbe8;--accent-coral:#e9a2ac;--accent-sunbeam:#d4b56a;color-scheme:dark}
    *{box-sizing:border-box}html{background:var(--surface-base);color:var(--text-primary);font-family:"Atkinson Hyperlegible Next","Atkinson Hyperlegible",Verdana,sans-serif;font-size:100%}body{margin:0;min-width:20rem;background:var(--surface-base);font-size:var(--type-body);line-height:1.55}button,a,input,select{font:inherit}button,a{min-height:2.75rem}.skip{position:fixed;left:var(--space-md);top:-5rem;z-index:20;background:var(--action-primary);color:var(--action-on-primary);padding:var(--space-sm) var(--space-md);border-radius:var(--radius-md)}.skip:focus{top:var(--space-md)}:focus-visible{outline:.2rem solid var(--focus-ring);outline-offset:.2rem}button{border:1px solid var(--border-line-strong);background:var(--surface-raised);color:var(--text-primary);border-radius:var(--radius-full);padding:.65rem 1rem;cursor:pointer;font-weight:600;letter-spacing:.02em}button:hover{background:var(--surface-sunken)}button:active{transform:translateY(.0625rem)}button[disabled]{cursor:not-allowed;opacity:.56}.primary{background:var(--action-primary);color:var(--action-on-primary);border-color:var(--action-primary)}.primary:hover{background:var(--text-primary);color:var(--surface-base);border-color:var(--text-primary)}.danger{border-color:var(--status-failed);color:var(--status-failed)}.text-button{border-color:transparent;background:transparent;text-decoration:underline}.icon-button{width:2.75rem;padding:0;display:grid;place-items:center}h1,h2,h3{font-family:"Young Serif",Georgia,serif;font-weight:400;line-height:1.2;margin:0}h1{font-size:calc(var(--type-h1) * 1.65);letter-spacing:-.02em}h2{font-size:calc(var(--type-h1) * 1.25);letter-spacing:-.01em}h3{font-size:var(--type-lead)}p{margin:.35rem 0}.muted{color:var(--text-soft)}.micro{font-size:var(--type-micro);letter-spacing:.02em}.label{font-size:var(--type-label);font-weight:600}.sr-only{position:absolute;width:1px;height:1px;padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;border:0}.topbar{height:4.5rem;display:flex;align-items:center;gap:var(--space-md);padding:0 var(--space-lg);border-bottom:1px solid var(--border-line);background:var(--surface-raised);position:sticky;top:0;z-index:8}.brand{display:flex;align-items:center;gap:.65rem;margin-right:auto}.brand h1{font-size:var(--type-h1)}.mockup{font-size:var(--type-micro);font-weight:700;letter-spacing:.09em;border:1px solid var(--accent-mint);color:var(--accent-mint);padding:.2rem .45rem;border-radius:var(--radius-sm)}.app{display:grid;grid-template-columns:13.5rem minmax(0,1fr);min-height:calc(100dvh - 4.5rem)}.rail{padding:var(--space-lg) var(--space-md);border-right:1px solid var(--border-line);background:var(--surface-sunken)}.nav-btn{width:100%;border:0;border-radius:var(--radius-md);text-align:left;background:transparent;display:flex;align-items:center;gap:.75rem;margin-bottom:.35rem}.nav-btn[aria-current="page"]{background:var(--surface-raised);box-shadow:inset .2rem 0 0 var(--action-primary)}.nav-glyph{font-size:1.2rem;width:1.5rem;text-align:center}.main{padding:var(--space-xl);max-width:92rem;width:100%;margin:0 auto}.page-head{display:flex;align-items:flex-end;justify-content:space-between;gap:var(--space-lg);margin-bottom:var(--space-xl)}.status-line{display:flex;gap:.45rem;align-items:center;flex-wrap:wrap;color:var(--text-soft)}.status-word{font-weight:700;color:var(--status-needs-you)}.counter{display:grid;grid-template-columns:minmax(17rem,23rem) minmax(0,1fr);gap:var(--space-xl);align-items:start}.slips{display:grid;gap:var(--space-md)}.slip{width:100%;text-align:left;border-radius:var(--radius-lg);padding:var(--space-lg);background:var(--surface-raised);position:relative;overflow:hidden;transition:transform 150ms cubic-bezier(.2,0,0,1),border-color 150ms}.slip:hover,.slip[aria-pressed="true"]{transform:translateY(-.125rem);border-color:var(--action-primary)}.slip-top{display:flex;justify-content:space-between;gap:var(--space-sm);align-items:start}.state{font-size:var(--type-small);font-weight:700}.state.healthy{color:var(--status-healthy)}.state.needs_attention,.state.stale{color:var(--status-needs-you)}.state.unavailable{color:var(--status-unavailable)}.state.unknown,.state.not_configured{color:var(--status-unknown)}.fact-peek{margin-top:var(--space-md);padding-top:var(--space-md);border-top:1px solid var(--border-line);display:flex;justify-content:space-between;gap:var(--space-sm)}.detail{min-width:0}.receipt{background:var(--surface-raised);border:2px solid var(--border-line-strong);border-radius:var(--radius-lg);padding:var(--space-xl)}.receipt-head{display:flex;align-items:start;justify-content:space-between;gap:var(--space-lg);margin-bottom:var(--space-lg)}.receipt-stations{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:var(--space-md)}.station{background:var(--surface-sunken);border-radius:var(--radius-md);padding:var(--space-md);min-height:8.5rem;opacity:1;transition:opacity 150ms cubic-bezier(.2,0,0,1),transform 150ms cubic-bezier(.2,0,0,1)}.station.updated{animation:settle 360ms cubic-bezier(.2,0,0,1)}.station-title{font-weight:700;margin-bottom:var(--space-sm)}.permission{display:block;margin-top:.45rem;font-size:var(--type-small)}.app-frame{margin-top:var(--space-lg);background:var(--surface-raised);border:1px solid var(--border-line);border-radius:var(--radius-lg);overflow:hidden}.frame-bar{display:flex;align-items:center;justify-content:space-between;gap:var(--space-md);padding:var(--space-md);border-bottom:1px solid var(--border-line)}iframe{width:100%;height:15rem;border:0;background:var(--surface-sunken)}.frame-fallback{padding:var(--space-md);color:var(--text-soft)}.frame-fallback a,.inline-link{color:var(--text-primary);text-decoration-thickness:.1rem;text-underline-offset:.2rem}.dot-line{display:flex;gap:var(--space-md);align-items:center;margin-top:var(--space-lg);color:var(--text-soft)}.dot{width:3.25rem;height:2.5rem;flex:none}.dot .shell{fill:var(--accent-apricot);stroke:var(--surface-base);stroke-width:1.5}.dot .body{fill:var(--accent-mint);stroke:var(--surface-base);stroke-width:1.5}.dot .ink{stroke:var(--surface-base);stroke-width:1.5;stroke-linecap:round;fill:none}.dot .slip-paper{fill:var(--surface-raised);stroke:var(--border-line-strong);stroke-width:1}.updated-note{color:var(--status-done);font-weight:700}.facts-list{display:grid;gap:var(--space-md)}.fact-row{padding:var(--space-lg);border:1px solid var(--border-line);border-radius:var(--radius-lg);background:var(--surface-raised);display:grid;grid-template-columns:minmax(0,1.2fr) minmax(0,.8fr);gap:var(--space-md)}.fact-value{font-size:var(--type-lead);font-weight:700}.owner-action{margin-top:var(--space-md);display:flex;gap:var(--space-sm);align-items:center;flex-wrap:wrap}.mobile-tabs,.mobile-back,.bottom-nav,.fab{display:none}.settings-grid{display:grid;grid-template-columns:14rem minmax(0,1fr);gap:var(--space-xl)}.settings-nav{position:sticky;top:6rem;align-self:start}.settings-nav a{display:block;color:var(--text-soft);padding:.7rem;text-decoration:none}.settings-section{padding:var(--space-xl);background:var(--surface-raised);border-radius:var(--radius-lg);margin-bottom:var(--space-lg)}.setting-row{display:flex;justify-content:space-between;align-items:center;gap:var(--space-md);padding:var(--space-md) 0;border-bottom:1px solid var(--border-line)}.setting-row:last-child{border-bottom:0}.reference{overflow-wrap:anywhere}.radio-row{display:flex;gap:var(--space-sm);flex-wrap:wrap}.toast{position:fixed;right:var(--space-lg);bottom:var(--space-lg);max-width:28rem;background:var(--surface-raised);border:2px solid var(--status-done);border-radius:var(--radius-lg);padding:var(--space-md) var(--space-lg);z-index:30;box-shadow:0 1rem 3rem color-mix(in oklch,var(--surface-sunken) 70%,transparent);transform:translateY(1rem);opacity:0;pointer-events:none;transition:opacity 150ms,transform 150ms}.toast.show{transform:none;opacity:1}.modal{border:0;padding:0;background:transparent;color:var(--text-primary);width:min(42rem,calc(100% - 2rem));max-height:calc(100dvh - 2rem)}.modal::backdrop{background:color-mix(in oklch,var(--surface-sunken) 80%,transparent)}.modal-card{background:var(--surface-raised);border:2px solid var(--border-line-strong);border-radius:var(--radius-lg);padding:var(--space-xl);max-height:calc(100dvh - 2rem);overflow:auto}.modal-actions{display:flex;justify-content:flex-end;gap:var(--space-sm);margin-top:var(--space-xl)}.step-progress{display:flex;gap:.35rem;margin:var(--space-lg) 0}.step-progress span{height:.25rem;flex:1;background:var(--border-line);border-radius:var(--radius-full)}.step-progress span.done{background:var(--action-primary)}.discover-card{display:flex;justify-content:space-between;align-items:center;gap:var(--space-md);padding:var(--space-lg);background:var(--surface-sunken);border-radius:var(--radius-lg)}.field{display:grid;gap:var(--space-sm)}input,select{width:100%;min-height:2.75rem;border:1px solid var(--border-line-strong);border-radius:var(--radius-md);padding:.7rem;background:var(--surface-sunken);color:var(--text-primary)}.preview-facts{display:grid;gap:var(--space-sm)}.preview-facts label{display:grid;grid-template-columns:1.5rem 1fr;gap:var(--space-sm);padding:var(--space-md);background:var(--surface-sunken);border-radius:var(--radius-md)}.welcome{display:grid;grid-template-columns:minmax(0,1.3fr) minmax(14rem,.7fr);gap:var(--space-xl);padding:var(--space-2xl);background:var(--surface-raised);border:2px solid var(--border-line-strong);border-radius:var(--radius-lg)}.welcome-copy p{max-width:55ch}.welcome-dot{display:grid;place-items:center;background:var(--surface-sunken);border-radius:var(--radius-lg);min-height:15rem}.welcome-dot .dot{width:10rem;height:auto}.empty{padding:var(--space-xl);border:1px dashed var(--border-line-strong);border-radius:var(--radius-lg);color:var(--text-soft)}@keyframes settle{0%{opacity:.45;transform:translateY(.4rem)}100%{opacity:1;transform:none}}
    @media(max-width:60rem){.app{grid-template-columns:4.75rem minmax(0,1fr)}.rail{padding:var(--space-md) .65rem}.nav-btn{justify-content:center}.nav-label{position:absolute;left:4.4rem;background:var(--surface-raised);padding:.4rem .7rem;border-radius:var(--radius-md);opacity:0;pointer-events:none}.nav-btn:focus .nav-label,.nav-btn:hover .nav-label{opacity:1}.counter{grid-template-columns:1fr}.slips{grid-template-columns:repeat(2,minmax(0,1fr))}.detail{grid-column:1/-1}.topbar .desktop-label{display:none}}
    @media(max-width:47.99rem){body{padding-bottom:calc(3.5rem + env(safe-area-inset-bottom))}.topbar{height:auto;min-height:4.5rem;padding:var(--space-sm) var(--space-md);display:grid;grid-template-columns:1fr auto}.topbar-actions{grid-column:1/-1;display:grid;grid-template-columns:1fr 1fr;gap:var(--space-sm)}.topbar>#add-service,.topbar>#disconnect{display:none}.app{display:block;min-height:0}.rail{display:none}.main{padding:var(--space-lg) var(--space-md) calc(6.5rem + env(safe-area-inset-bottom))}.page-head{display:block;margin-bottom:var(--space-lg)}.page-head h1{font-size:calc(var(--type-h1) * 1.35)}.status-line{margin-top:var(--space-sm)}.counter{display:block}.slips{display:grid;grid-template-columns:1fr;padding-bottom:5rem}.detail{display:none}.slip{min-height:8.5rem}.bottom-nav{position:fixed;left:0;right:0;bottom:0;z-index:9;display:grid;grid-template-columns:repeat(4,1fr);height:calc(3.5rem + env(safe-area-inset-bottom));padding-bottom:env(safe-area-inset-bottom);background:var(--surface-raised);border-top:1px solid var(--border-line)}.bottom-nav button{border:0;border-radius:0;background:transparent;padding:.35rem;font-size:var(--type-small)}.bottom-nav button[aria-current="page"]{box-shadow:inset 0 .2rem 0 var(--action-primary)}.fab{display:block;position:fixed;right:1rem;bottom:calc(3.5rem + env(safe-area-inset-bottom) + 1rem);z-index:10;box-shadow:0 .5rem 1.5rem color-mix(in oklch,var(--surface-sunken) 75%,transparent)}.mobile-service .bottom-nav,.connect-open .bottom-nav{display:none}.mobile-service .fab{display:none}.mobile-service .detail{display:block}.mobile-service .slips,.mobile-service .page-head{display:none}.mobile-back{display:inline-flex;margin-bottom:var(--space-md)}.mobile-tabs{display:grid;grid-template-columns:repeat(3,1fr);gap:.25rem;margin:var(--space-md) 0}.mobile-tabs button{border-radius:var(--radius-md);padding:.55rem}.mobile-tabs button[aria-selected="true"]{background:var(--action-primary);color:var(--action-on-primary)}.receipt{padding:var(--space-lg)}.receipt-head{display:block}.receipt-head .danger{margin-top:var(--space-md)}.receipt-stations{grid-template-columns:1fr}.app-frame{display:none}.mobile-panel[data-active="app"] .receipt,.mobile-panel[data-active="app"] .dot-line{display:none}.mobile-panel[data-active="app"] .app-frame{display:block}.mobile-panel[data-active="agent"] .receipt-stations,.mobile-panel[data-active="agent"] .app-frame{display:none}.mobile-panel[data-active="agent"] .receipt:after{content:"Agent facts use the same shared fact store. Open Memory for every service.";display:block;color:var(--text-soft)}.settings-grid{display:block}.settings-nav{display:none}.settings-section{padding:var(--space-lg)}.setting-row{align-items:flex-start;flex-direction:column}.welcome{grid-template-columns:1fr;padding:var(--space-lg)}.welcome-dot{order:-1;min-height:10rem}.welcome-dot .dot{width:7rem}.modal{width:100%;max-height:100dvh;margin:0;align-self:end}.modal-card{border-radius:var(--radius-lg) var(--radius-lg) 0 0;max-height:100dvh;padding:var(--space-lg)}.connect-dialog .modal-card{border-radius:0;min-height:100dvh;display:flex;flex-direction:column}.connect-dialog .modal-actions{margin-top:auto;position:sticky;bottom:0;background:var(--surface-raised);padding-top:var(--space-md)}.fact-row{grid-template-columns:1fr}.toast{left:var(--space-md);right:var(--space-md);bottom:calc(4.5rem + env(safe-area-inset-bottom))}}
    @media(prefers-reduced-motion:reduce){*,*::before,*::after{scroll-behavior:auto!important;animation:none!important;transition-duration:.01ms!important}.slip:hover{transform:none}}
  </style>
</head>
<body>
  <a class="skip" href="#main">Skip to Worlds</a>
  <header class="topbar" data-od-id="worlds-header">
    <div class="brand"><span class="mockup">MOCKUP</span><h1>Worlds</h1></div>
    <div class="topbar-actions"><button id="live-toggle" aria-pressed="true">Live · checked 24s ago</button><button id="simulate-change">Simulate change</button></div>
    <button id="add-service" class="primary">Add service</button><button id="disconnect" class="danger">Disconnect</button>
  </header>
  <div class="app" id="app-shell">
    <nav class="rail" aria-label="Worlds sections">
      <button class="nav-btn" data-view="home" aria-current="page"><span class="nav-glyph">H</span><span class="nav-label">Home</span></button>
      <button class="nav-btn" data-view="connect"><span class="nav-glyph">C</span><span class="nav-label">Connect</span></button>
      <button class="nav-btn" data-view="memory"><span class="nav-glyph">M</span><span class="nav-label">Memory</span></button>
      <button class="nav-btn" data-view="settings"><span class="nav-glyph">S</span><span class="nav-label">Settings</span></button>
    </nav>
    <main class="main" id="main" tabindex="-1"></main>
  </div>
  <nav class="bottom-nav" aria-label="Worlds mobile sections">
    <button data-view="home" aria-current="page">Home</button><button data-view="connect">Connect</button><button data-view="memory">Memory</button><button data-view="settings">Settings</button>
  </nav>
  <button class="fab primary" id="mobile-add">Add service</button>
  <div class="toast" id="toast" role="status" aria-live="polite"></div>
  <dialog class="modal connect-dialog" id="connect-dialog"><div class="modal-card" id="connect-content"></div></dialog>
  <dialog class="modal" id="disconnect-dialog"><form method="dialog" class="modal-card"><h2 id="disconnect-title">Disconnect service?</h2><p id="disconnect-copy"></p><div class="modal-actions"><button value="cancel">Keep service</button><button value="confirm" class="danger" id="confirm-disconnect">Disconnect</button></div></form></dialog>
  <script>
  const fixture={generatedAt:'2026-10-13T14:24:26Z',services:{
    'media-shelf':{id:'media-shelf',name:'Media Shelf',kind:'media library',state:'healthy',stateLabel:'Healthy',summary:'Library reachable and indexing is current.',owner:{app:'Media',appRoute:'app://media',fallback:'Open Media app'},checkedAt:'2026-10-13T14:23:58Z',source:'local',facts:[['media.items','Items in library',1284,'items','live','read','read'],['media.indexing','Indexing backlog',0,'items','live','read','read'],['media.played-recently','Played in the last 7 days',12,'items','live','read','read']]},
    'front-door':{id:'front-door',name:'Front Door',kind:'home access',state:'needs_attention',stateLabel:'Needs a human',summary:"Back door has been unlocked for longer than the person's usual window.",reason:'A decision is required; the local app will not act on its own.',owner:{app:'Home',appRoute:'app://home',fallback:'Open Home app'},checkedAt:'2026-10-13T14:24:02Z',source:'local',facts:[['frontdoor.back-door','Back door','unlocked',null,'live','read','act',null,"Locking the door changes the external system and is the person's decision."],['frontdoor.unlocked-for','Unlocked for',47,'minutes','live','read','read'],['frontdoor.guest-code-used','Guest code last used by','visitor-3',null,'live','none','read','Identifying a person is human-only in this fixture; the agent sees that a guest code was used, not who.']]},
    'notes-sync':{id:'notes-sync',name:'Notes Sync',kind:'sync',state:'unavailable',stateLabel:'Unavailable',summary:'The sync provider did not answer its last two checks. Local notes still work; retry is offered.',owner:{app:'Memory',appRoute:'app://memory',fallback:'Open Memory app'},checkedAt:'2026-10-13T14:20:00Z',source:'none',lastError:'provider timed out',facts:[['sync.provider-reachable','Provider reachable',null,null,'unavailable','read','read'],['sync.local-pending','Local notes waiting to upload',3,'notes','live','read','read'],['sync.last-synced','Last successful sync','2026-10-13T13:58:00Z',null,'stale','read','read']]},
    'local-weather':{id:'local-weather',name:'Local Weather',state:'not_configured',stateLabel:'Not configured',summary:'No weather provider has ever been set up. The tile holds zero data until the person chooses one; nothing is guessed.',emptyReason:'No provider configured, so there are no facts to show. The concept must render the empty state honestly rather than a placeholder value.',owner:{app:'Settings',appRoute:'app://settings/providers',fallback:'Open provider settings'},facts:[]}
  }};
  const store={now:new Date(fixture.generatedAt),live:true,cadence:5,view:'home',connected:['media-shelf','front-door'],selected:'front-door',references:{'media-shelf':'ref://media-shelf','front-door':'ref://front-door'},connectStep:0,connectRef:'ref://notes-sync',chosen:new Set(['sync.provider-reachable','sync.local-pending','sync.last-synced']),mobilePanel:'facts',updated:false,staleFired:false,disconnectedMessage:''};
  const $=s=>document.querySelector(s),$$=s=>[...document.querySelectorAll(s)];
  function esc(v){return String(v).replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]))}
  function age(id){const s=fixture.services[id];if(!s.checkedAt)return 'never checked';const n=Math.max(0,Math.round((store.now-new Date(s.checkedAt))/1000));if(n<60)return `${n}s ago`;if(n<3600)return `${Math.floor(n/60)}m ago`;return `${Math.floor(n/3600)}h ago`}
  function value(f){if(f[2]===null)return f[4]==='unknown'?'Unknown':'Unavailable';return `${f[2]}${f[3]?' '+f[3]:''}`}
  function dot(carry=false){return `<svg class="dot" viewBox="0 0 96 72" role="img" aria-label="Dot the snail${carry?' carrying a service slip':''}"><path class="body" d="M14 51c4-10 14-14 27-12 9 1 15 8 24 8h16c7 0 10 4 10 9H18c-6 0-8-2-4-5Z"/><circle class="shell" cx="45" cy="35" r="21"/><path class="ink" d="M45 24c9 0 13 8 9 14-4 6-14 4-14-3 0-4 3-6 7-6M75 46c0-8 4-13 8-16m-8 16c-1-7-5-11-9-14M84 29h1M65 31h1"/>${carry?'<path class="slip-paper" d="M8 13h28v22H8z"/><path class="ink" d="M13 19h17m-17 6h13"/>':''}</svg>`}
  function attention(){return store.connected.filter(id=>fixture.services[id].state==='needs_attention').length}
  function selected(){return fixture.services[store.selected]||fixture.services[store.connected[0]]}
  function announce(msg){const t=$('#toast');t.textContent=msg;t.classList.add('show');clearTimeout(announce.t);announce.t=setTimeout(()=>t.classList.remove('show'),3600)}
  function slip(s){const f=s.facts[0];return `<button class="slip" data-service="${s.id}" aria-pressed="${s.id===store.selected}"><span class="slip-top"><span><span class="label">${s.name}</span><span class="micro muted" style="display:block">${s.kind}</span></span><span class="state ${s.state}">${s.stateLabel}</span></span><span class="muted" style="display:block;margin-top:.55rem">${s.reason||s.summary}</span><span class="fact-peek"><span>${f?f[1]:'No facts'}</span><strong>${f?value(f):'None'}</strong></span><span class="micro muted" style="display:block;margin-top:.5rem">Checked ${age(s.id)}</span></button>`}
  function receipt(s){const f=s.facts[0]||['','No facts',null,null,'unknown','none','none'];const reason=f[7]||f[8]||s.reason||s.summary;return `<section class="receipt" data-od-id="fact-receipt"><div class="receipt-head"><div><p class="micro muted">FACT JOURNEY</p><h2>${f[1]}</h2><p class="fact-value">${value(f)}</p></div><button class="danger mobile-disconnect">Disconnect</button></div><div class="receipt-stations"><div class="station ${store.updated?'updated':''}"><p class="station-title">Source</p><p>${s.owner.app} reported <strong>${value(f)}</strong>.</p><span class="permission">State: ${f[4]}</span><span class="permission">Checked ${age(s.id)}</span></div><div class="station ${store.updated?'updated':''}"><p class="station-title">Worlds</p><p>Shared as ${f[1].toLowerCase()}.</p><span class="permission">Connected during first run · reference held, never the secret</span></div><div class="station ${store.updated?'updated':''}"><p class="station-title">Agent / Human</p><span class="permission">Agent can: ${f[5]}</span><span class="permission">Human can: ${f[6]}</span>${reason?`<span class="permission muted">${reason}</span>`:''}</div></div>${f[6]==='act'?`<div class="owner-action"><button class="primary handoff">Hand off to ${s.owner.app}</button><span class="muted">Worlds does not lock the door.</span></div>`:''}</section>`}
  function frame(s){return `<section class="app-frame" data-od-id="embedded-app-frame"><div class="frame-bar"><strong>${s.owner.app} app</strong><a class="inline-link" href="${s.owner.appRoute}">${s.owner.fallback}</a></div><iframe title="Embedded ${s.owner.app} app" srcdoc="<style>body{margin:0;padding:28px;background:#1f1625;color:#f5ecf2;font:16px Verdana,sans-serif}small{color:#dacad9}section{border:1px solid #8b7791;border-radius:12px;padding:20px}a{color:#a7e6ce}</style><section><small>OWNED BY ${s.owner.app.toUpperCase()}</small><h2>${s.name}</h2><p>${esc(s.summary)}</p><p>This contained preview receives no Worlds credentials.</p></section>"></iframe><div class="frame-fallback">If this app does not load here: <a href="https://example.invalid/open/${s.id}">open in the owning app</a>. <button class="text-button retry-frame">Retry</button></div></section>`}
  function detail(s){return `<div class="detail mobile-panel" data-active="${store.mobilePanel}"><button class="mobile-back">Back to counter</button><div class="mobile-tabs" role="tablist"><button role="tab" data-tab="facts" aria-selected="${store.mobilePanel==='facts'}">Facts</button><button role="tab" data-tab="app" aria-selected="${store.mobilePanel==='app'}">App</button><button role="tab" data-tab="agent" aria-selected="${store.mobilePanel==='agent'}">Agent view</button></div>${receipt(s)}${frame(s)}<div class="dot-line">${dot()}<span>${store.updated?'<strong class="updated-note">Updated just now.</strong> ':''}Dot rests beside the freshness line. Checked ${age(s.id)}.</span></div></div>`}
  function home(){if(!store.connected.length)return `<div class="page-head"><div><h1>Your counter</h1><p class="status-line">No services are connected yet.</p></div></div><section class="welcome"><div class="welcome-copy"><p class="micro muted">A SMALL WELCOME</p><h2>Facts can arrive without losing their owner.</h2><p>Connect a service. Dot will carry the first slip, and Worlds will show how one fact travels.</p><button class="primary start-connect">Connect a service</button></div><div class="welcome-dot">${dot(true)}</div></section>`;const s=selected();return `<div class="page-head"><div><h1>Your counter</h1><p class="status-line"><span class="status-word">${attention()?`${attention()} needs you`:'Nothing needs you right now'}</span><span>· checked ${age('front-door')}</span></p></div><p class="muted">Choose a slip. The receipt keeps source, sharing and authority visible.</p></div><div class="counter"><section class="slips" aria-label="Connected service slips">${store.connected.map(id=>slip(fixture.services[id])).join('')}</section>${detail(s)}</div>`}
  function memory(){const rows=store.connected.flatMap(id=>fixture.services[id].facts.map(f=>({s:fixture.services[id],f})));return `<div class="page-head"><div><h1>Agent facts</h1><p class="muted">One shared store, with visibility and authority stated separately.</p></div></div><section class="facts-list">${rows.map(({s,f})=>`<article class="fact-row"><div><h3>${f[1]}</h3><p class="fact-value">${value(f)}</p><p class="micro muted">${s.name} · source ${s.source} · observed ${age(s.id)}</p></div><div><span class="state ${f[4]}">${f[4]}</span><p>Agent can: <strong>${f[5]}</strong></p><p>Human can: <strong>${f[6]}</strong></p>${f[7]?`<p class="muted">${f[7]}</p>`:''}</div></article>`).join('')||'<div class="empty">No agent facts yet. Connect a service to add facts without guessing.</div>'}</section>`}
  function settings(){return `<div class="page-head"><div><h1>Settings</h1><p class="muted">Worlds keeps references and leaves secrets with their owners.</p></div></div><div class="settings-grid"><nav class="settings-nav" aria-label="Settings sections"><a href="#connected-services">Connected services</a><a href="#credential-references">Credential references</a><a href="#providers">Providers</a><a href="#live-feed">Live feed</a><a href="#about">About</a></nav><div><section class="settings-section" id="connected-services"><h2>Connected services</h2>${store.connected.map(id=>{const s=fixture.services[id];return `<div class="setting-row"><div><strong>${s.name}</strong><p class="muted">${s.stateLabel} · ${age(id)}</p></div><button class="danger row-disconnect" data-id="${id}">Disconnect</button></div>`}).join('')||'<p class="empty">Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you.</p>'}</section><section class="settings-section" id="credential-references"><h2>Credential references</h2><p class="muted">Worlds stores a credential reference, never the secret.</p>${Object.entries(store.references).map(([id,ref])=>`<div class="setting-row"><div><strong>${fixture.services[id].name}</strong><p class="reference">${esc(ref)}</p></div><div><button class="replace-ref" data-id="${id}">Replace</button> <button class="remove-ref" data-id="${id}">Remove</button></div></div>`).join('')||'<p>No references stored. Connecting a service will add one.</p>'}</section><section class="settings-section" id="providers"><h2>Providers</h2><div class="setting-row"><div><strong>Local Weather</strong><p class="state not_configured">Not configured</p><p>${fixture.services['local-weather'].summary}</p><p class="muted">${fixture.services['local-weather'].emptyReason}</p></div><a class="inline-link" href="app://settings/providers">Open provider settings</a></div></section><section class="settings-section" id="live-feed"><h2>Live feed</h2><div class="setting-row"><div><strong>Updates</strong><p class="muted">Pausing freezes freshness. Actions still work.</p></div><button id="settings-live" aria-pressed="${store.live}">${store.live?'On':'Paused'}</button></div><div class="setting-row"><div><strong>Check cadence</strong></div><select id="cadence"><option value="5" ${store.cadence===5?'selected':''}>5 seconds</option><option value="10" ${store.cadence===10?'selected':''}>10 seconds</option><option value="30" ${store.cadence===30?'selected':''}>30 seconds</option></select></div><div class="setting-row"><div><strong>Reduce motion</strong><p class="muted">Reduced motion removes animation, not data.</p></div><select><option>Follow system</option><option>Always</option></select></div></section><section class="settings-section" id="about"><h2>About</h2><p><span class="mockup">MOCKUP</span> Synthetic data only.</p><p class="muted">UNKNOWN: production Connect API, reference semantics, embedded app sandbox support, disconnect history and live transport.</p><button disabled>Keep history after disconnect</button><p class="muted">Not decided yet; this mockup keeps nothing.</p></section></div></div>`}
  function render(){const main=$('#main');main.innerHTML=store.view==='home'?home():store.view==='memory'?memory():store.view==='settings'?settings():home();$('#app-shell').classList.toggle('mobile-service',!!store.mobileService);$$('[data-view]').forEach(b=>b.setAttribute('aria-current',b.dataset.view===store.view?'page':'false'));const s=selected();$('#disconnect').disabled=!s;$('#disconnect').title=s?'': 'Connect a service before disconnecting';$('#live-toggle').setAttribute('aria-pressed',store.live);$('#live-toggle').textContent=store.live?`Live · checked ${store.connected.includes('front-door')?age('front-door'):'now'}`:`Paused · frozen at ${store.connected.includes('front-door')?age('front-door'):'now'}`;bind();}
  function bind(){$$('.slip').forEach(b=>b.onclick=()=>{store.selected=b.dataset.service;store.mobileService=matchMedia('(max-width: 47.99rem)').matches;store.mobilePanel='facts';render()});$$('.nav-btn,.bottom-nav button').forEach(b=>b.onclick=()=>navigate(b.dataset.view));$$('.start-connect').forEach(b=>b.onclick=openConnect);$$('.mobile-back').forEach(b=>b.onclick=()=>{store.mobileService=false;render()});$$('[data-tab]').forEach(b=>b.onclick=()=>{store.mobilePanel=b.dataset.tab;render()});$$('.mobile-disconnect').forEach(b=>b.onclick=openDisconnect);$$('.handoff').forEach(b=>b.onclick=()=>{announce('Mock handoff recorded; Worlds locked nothing. Opening Home.');location.hash='owner-home'});$$('.retry-frame').forEach(b=>b.onclick=()=>announce("This app didn't load inside Worlds. Facts and the owner link still work."));$$('.row-disconnect').forEach(b=>b.onclick=()=>{store.selected=b.dataset.id;openDisconnect()});$('#settings-live')?.addEventListener('click',toggleLive);$('#cadence')?.addEventListener('change',e=>{store.cadence=Number(e.target.value);announce(`Live feed now checks every ${store.cadence} seconds.`)});$$('.remove-ref').forEach(b=>b.onclick=()=>{delete store.references[b.dataset.id];fixture.services[b.dataset.id].state='not_configured';fixture.services[b.dataset.id].stateLabel='Not configured';announce(`${fixture.services[b.dataset.id].name} reference removed. Reconnect to restore it.`);render()});$$('.replace-ref').forEach(b=>b.onclick=()=>{store.connectRef='';openConnect(1)})}
  function navigate(v){if(v==='connect'){openConnect();return}store.view=v;store.mobileService=false;render();$('#main').focus()}
  function toggleLive(){store.live=!store.live;announce(store.live?'Live feed resumed.':'Live feed paused. Freshness is frozen.');render()}
  function simulate(){const s=fixture.services['front-door'];if(!store.connected.includes('front-door')){announce('Front Door is not connected, so there is no shared fact to update.');return}s.facts[0][2]='locked';s.state='healthy';s.stateLabel='Healthy';s.summary='Back door is locked. The local app remains the owner.';s.checkedAt=store.now.toISOString();store.updated=true;announce('Updated across Home, Front Door and Agent facts. Mock handoff recorded; Worlds locked nothing.');render();setTimeout(()=>{store.updated=false;render()},3000)}
  function openDisconnect(){const s=selected();if(!s)return;$('#disconnect-title').textContent=`Disconnect ${s.name}?`;$('#disconnect-copy').textContent=`Its Worlds facts and app frame will be removed. The ${s.owner.app} app remains untouched.`;$('#disconnect-dialog').showModal()}
  $('#disconnect-dialog').addEventListener('close',()=>{if($('#disconnect-dialog').returnValue!=='confirm')return;const s=selected();store.connected=store.connected.filter(id=>id!==s.id);delete store.references[s.id];store.selected=store.connected[0]||null;store.mobileService=false;store.disconnectedMessage=`${s.name} removed. Its Worlds facts and frame are gone. The ${s.owner.app} app remains.`;announce(store.disconnectedMessage);render()});
  const steps=['Discover','Reference','Test','Preview','Choose','Publish'];
  function openConnect(step=0){store.connectStep=step;document.body.classList.add('connect-open');renderConnect();$('#connect-dialog').showModal()}
  function renderConnect(){const s=fixture.services['notes-sync'];let body='';if(store.connectStep===0)body=`<div class="discover-card"><div><strong>${s.name}</strong><p>${s.summary}</p></div><span class="state unavailable">${s.stateLabel}</span></div>`;if(store.connectStep===1)body=`<div class="field"><label for="credential-ref">Credential reference</label><input id="credential-ref" value="${esc(store.connectRef)}" placeholder="ref://notes-sync" aria-describedby="ref-help"><p id="ref-help" class="muted">Enter a reference only. Never paste a secret.</p></div>`;if(store.connectStep===2)body=`<div class="welcome-dot">${dot(true)}<p>Safe test ready. No provider call will occur.</p></div><p><strong>Expected mock result:</strong> ${s.lastError}</p><button id="mock-fail">Show mock failure</button>`;if(store.connectStep===3)body=`<p>Three fixture facts are available. Unknown and unavailable values stay named.</p><div class="facts-list">${s.facts.map(f=>`<div class="fact-row"><div><strong>${f[1]}</strong><p>${value(f)}</p></div><span class="state ${f[4]}">${f[4]}</span></div>`).join('')}</div>`;if(store.connectStep===4)body=`<div class="preview-facts">${s.facts.map(f=>`<label><input type="checkbox" data-fact="${f[0]}" ${store.chosen.has(f[0])?'checked':''}><span><strong>${f[1]}</strong><br><span class="muted">${value(f)} · ${f[4]}</span></span></label>`).join('')}</div>`;if(store.connectStep===5)body=`<div class="welcome-dot">${dot(true)}<div><h3>Ready for the counter</h3><p>Publish adds Notes Sync and its chosen facts now.</p></div></div>`;$('#connect-content').innerHTML=`<p class="micro muted">CONNECT · ${steps[store.connectStep]}</p><h2>${steps[store.connectStep]} Notes Sync</h2><div class="step-progress" aria-label="Step ${store.connectStep+1} of 6">${steps.map((_,i)=>`<span class="${i<=store.connectStep?'done':''}"></span>`).join('')}</div>${body}<div class="modal-actions"><button id="connect-cancel">Cancel</button>${store.connectStep?'<button id="connect-back">Back</button>':''}<button class="primary" id="connect-next">${store.connectStep===5?'Publish':'Next'}</button></div>`;$('#connect-cancel').onclick=closeConnect;$('#connect-back')?.addEventListener('click',()=>{store.connectStep--;renderConnect()});$('#connect-next').onclick=nextConnect;$('#credential-ref')?.addEventListener('input',e=>store.connectRef=e.target.value);$$('[data-fact]').forEach(c=>c.onchange=()=>c.checked?store.chosen.add(c.dataset.fact):store.chosen.delete(c.dataset.fact));$('#mock-fail')?.addEventListener('click',()=>announce(`Test failed: ${s.lastError}. The reference was preserved; retry when ready.`))}
  function nextConnect(){if(store.connectStep===1&&!store.connectRef.startsWith('ref://')){announce('Use a credential reference beginning with ref://. No secret was stored.');return}if(store.connectStep<5){store.connectStep++;renderConnect();return}const s=fixture.services['notes-sync'];s.facts=s.facts.filter(f=>store.chosen.has(f[0]));if(!store.connected.includes(s.id))store.connected.push(s.id);store.references[s.id]=store.connectRef;store.selected=s.id;store.view='home';store.mobileService=false;render();announce('Connected Notes Sync. Its service slip and shared facts are now live.');$('#connect-content').innerHTML=`<p class="micro muted">CONNECTED</p><h2>Notes Sync is at the counter</h2><p>Its chosen facts now appear in Home, this service surface and Agent facts.</p><div class="dot-line">${dot(true)}<span>Reference held, never the secret. <a class="inline-link" href="#credential-references">Credential references</a></span></div><div class="modal-actions"><button id="receipt-more">Add one more</button><button class="primary" id="receipt-counter">See your counter</button></div>`;$('#receipt-more').onclick=()=>{store.connectStep=0;renderConnect()};$('#receipt-counter').onclick=closeConnect}
  function closeConnect(){document.body.classList.remove('connect-open');$('#connect-dialog').close()}
  $('#connect-dialog').addEventListener('cancel',e=>{e.preventDefault();closeConnect()});
  $('#add-service').onclick=openConnect;$('#mobile-add').onclick=openConnect;$('#disconnect').onclick=openDisconnect;$('#live-toggle').onclick=toggleLive;$('#simulate-change').onclick=simulate;
  let elapsed=0;setInterval(()=>{if(!store.live)return;store.now=new Date(store.now.getTime()+5000);elapsed+=5;if(elapsed%20===0&&store.connected.includes('front-door')&&fixture.services['front-door'].state==='healthy')fixture.services['front-door'].checkedAt=store.now.toISOString();if(elapsed%30===0&&store.connected.includes('media-shelf')&&fixture.services['media-shelf'].state==='healthy')fixture.services['media-shelf'].checkedAt=store.now.toISOString();if(elapsed>=90&&!store.staleFired&&store.connected.includes('media-shelf')){const s=fixture.services['media-shelf'];s.state='stale';s.stateLabel='Stale (simulated)';s.summary=`Nothing has changed since ${new Date(s.checkedAt).toLocaleTimeString([], {hour:'numeric',minute:'2-digit'})} that Worlds knows of.`;store.staleFired=true;announce('Media Shelf is stale. Its last facts remain visible with the checked time.')}render()},5000);
  render();
  </script>
</body>
</html>

```

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
