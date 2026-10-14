# Worlds vNext: critique the full design spec (another author's)

## Decisions in force
- 2026-10-13 Rylee: "Yeah. V2 is the only one that passed. Can we now feed that back in with all the field guide info and everything to generate a full design?"
- 2026-10-13 Rylee: "comprehensive but also keep the original feeling." Clarified: protect the feel, the look, the whimsy and the love, NOT the copy; copy may be rewritten for clarity.
- 2026-10-13 Rylee, Project Home decision 145: "Approve the boundaries; Sol's six conditions become pre-prototype gates"

Read `docs/vnext/full-design-brief.md`, `docs/vnext/v2/` and `docs/vnext/concept5/` (look at both index.html files), `docs/vnext/field-guide/`, `docs/vnext/house/` and `docs/vnext/connect/connect-contract.md`. Write `docs/vnext/worlds-design-CRITIQUE.md` with AT MOST 8 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Check hardest: (1) anything that dilutes the protected look, feel, whimsy and love of V2 and Concept 5 (Dot, the counter, the warmth, delight moments, motion character) or turns it into generic app screens; (2) missing screens or states from the brief's list; (3) Sol's six conditions not visible in the UI (fail-closed approval, inert proposals, scoped auth, read-only path first); (4) desktop/tablet/phone not each designed on purpose; (5) house tokens and Field Guide structure used correctly; (6) honesty (no invented data, stale/unknown in words). Copy wording is NOT protected; only flag copy that is unclear. Do not grade generously. Write only that file. Do not push.

---
## The spec

# Worlds vNext: the full design (MOCKUP spec)

Status: MOCKUP. This is a design, not an approved product and not deployed. It describes screens and behaviour; it contains no code. Data is the synthetic `docs/vnext/fixture/fixture.json` only.

## 1. The feeling to protect

Rylee's words: "comprehensive but also keep the original feeling", clarified as protect the feel, the look, the whimsy and the love, **not the copy**. This section is a review criterion for every later section. If something added below makes Worlds look like a generic admin console, it is wrong, however complete it is.

Sources studied: V2's `DESIGN.md` and `index.html` (the look as built: Mulberry & Mint), and Concept 5's `DESIGN.md` and `index.html` (where Dot, the stamp and the parcel were first drawn). Concept 5 was skinned in Wise lime and heavy type; that skin is **not** what Rylee picked. V2's Mulberry & Mint skin is. Where the two disagree, this spec follows V2's look and Concept 5's gestures.

### What makes it feel the way it does

**The look**

1. **A plum room with mint and apricot in it.** Everything sits on the three plum surfaces (`--surface-base` page, `--surface-raised` slips and cards, `--surface-sunken` wells and rails). Mint (`--action-primary`, `--accent-mint`) is the one thing to act on. Apricot (`--accent-apricot`, which is also the "needs you" colour) and lilac are accents, never fills. Seen in V2: the top bar, rail, slips, receipt, and the primary pills.
2. **Soft, round, paper-like shapes.** Slips and cards use `--radius-lg`; buttons and chips are full pills (`--radius-full`); inner wells use `--radius-md`. The receipt has a 2px `--border-line-strong` frame like a printed slip; empty states use a dashed `--border-line-strong` outline, like a space waiting for something. V2: `.slip`, `.receipt`, `.empty`, `.welcome`.
3. **Depth by surface, not by effect.** No gradients, no glow, no heavy shadows. Depth is the surface ladder plus a small lift: a slip rises about 2px and its border turns mint on hover or selection (150ms). Shadows exist only on things that float: the phone add-service button and the toast. V2: `.slip:hover`, `.fab`, `.toast`.
4. **Young Serif for the voice, Atkinson Hyperlegible Next for the reading.** The serif on headings gives the counter a printed, hand-labelled air; the body face keeps it readable. V2: every `h1`/`h2`/`h3`.

**The counter and the slips**

5. **Worlds is a service counter.** "Your counter" holds one **slip** per service: name, kind, state word, one-line reason, one peeked fact, "Checked 24s ago". You pick a slip and open its **receipt** (Source → Worlds → Agent / Human), three small sunken stations inside a framed card. That receipt is the daily hero. V2: `.slips`, `.receipt-stations`; Concept 5: `.st` stations.

**Dot, and how Dot behaves**

6. **Dot the snail is a small accent, never a gate and never the data courier.** Dot is drawn in line art with a spiral shell and eyestalks (Concept 5 `dot()`; V2's flatter apricot-shell, mint-body version). Fixed rules, taken from both concepts:
   - **Asleep** (closed eyes, a small "z") when the counter is empty: first run, after the last service is disconnected, empty lists. Concept 5 `dot('asleep')`, V2 §6.
   - **Nods once** (200ms) beside "Updated just now" when a service's state word changes. Never on a plain tick. Concept 5 `.dot-nod`.
   - **Carries the parcel** across when a service is published (400ms desktop, 300ms phone). V2 `dot(true)` carries a slip; Concept 5 `dot('parcel')` carries a ribboned parcel.
   - **Rests** beside the counter's freshness line on working screens, still.
   - Dot's face and pose never say whether something is good or bad. The words say that.

**The delight moments (the whimsy and the love)**

7. **The "Connected" stamp.** A tilted (about -4°) stamp in Young Serif capitals with a check lands beside Dot when a connection completes. Concept 5 `.stamp`, kept in V2 §11. It is the one flourish allowed to be a little proud.
8. **The parcel hand-off.** The Publish step is not a spinner; Dot walks the parcel to the counter and the new slip appears. Concept 5 `phase === 'delivering'`.
9. **The quiet calm.** When nothing needs a person the counter says so in one plain line ("Nothing needs you right now") and Dot rests. No confetti, no streak, no score. V2 `home()` status line.
10. **The receipt settling.** When a fact changes, the three stations tick in order, Source then Worlds then Agent / Human, 60ms apart. It is the counter showing its work. V2 `.station.updated`, Concept 5 §15.
11. **Kind small print.** Care shows up as a provenance line ("Connected during first run · reference held, never the secret"), as the service-drop line ("That service stopped answering. Its last facts are still here, marked with when Worlds checked."), and as "Worlds locked nothing." They sound like a person who understands what is happening wrote them. The wording is free to change; the habit of saying them is protected.

**The pacing**

12. **An unhurried, skippable welcome, one thing at a time.** V2 opens on a single broad welcome card with a sleeping Dot, one invitation and one button, with "Skip welcome" always available; it earns its way to the counter one connection at a time. Nothing is a tour you must finish.

**The motion character**

13. **Gentle, one curve, and only to confirm.** One easing, `cubic-bezier(.2,0,0,1)`. Motion confirms a change that has already happened; nothing moves on a routine tick except the age number. Under reduced motion the same moments become opacity-only or instant, and data never stops. V2 §11.

### How to use this section
- **Copy is not protected.** Every string in §7 is rewritten for clarity using `docs/vnext/house/LANGUAGE.md` (machine truth, then plain words, then optional voice). The quoted V2 lines above are examples of the habit, not required wording.
- **Coverage must not flatten the feeling.** Each new screen in §4 must name which of items 1-13 it carries. Where a screen is consequential (Needs You, disconnect confirm, revoked credential) it is deliberately *quiet*: no Dot, no flourish. `JOY-CURIOSITY-AND-CARE.md` says operational and consequential surfaces may be very quiet, and gifts are not toll booths.
- **Ignorable.** A person can ignore every flourish (Dot, stamp, parcel) and finish every job.

## 2. What this design is

One Worlds, one flow, from an empty counter to a counter that keeps working: welcome, connect a service, preview it, publish it, watch facts change and age, follow a link out to the owning system, disconnect, and be back at an honest empty counter.

**Chosen base: V2 "Welcome-led Counter"** (Rylee: "V2 is the only one that passed"), grown to the full application using the Field Guide structure, the Rylee-Bee house tokens (Mulberry & Mint), and the Connect contract with Sol's six pre-prototype conditions.

### Deliberate changes from V2 (so reviewers can find them)
| # | Change | Why |
|---|---|---|
| 1 | Connect is a **destination page** (desktop, tablet) and a full-screen stepper (phone), not a modal dialog. | A real application needs back-button, deep links and focus order that a modal makes awkward. V2 and Concept 5 already agree Connect is a destination in the nav. |
| 2 | **Needs you** is its own destination. | The Field Guide skeleton names it; it is also where the Project Home handoff lives (Sol condition 1). |
| 3 | The line "Mock handoff recorded; Worlds locked nothing." and the "Hand off to Home" button are replaced. | "Handoff recorded" reads as if an approval path works. It does not yet (condition 1). Replacement: an **owner link** ("Open Home app") and a separate **approval handoff** that honestly says "Approvals aren't connected yet." |
| 4 | Disconnect **keeps the credential reference** unless the person ticks "Also remove the reference". | The Connect contract's `disconnect` retains the reference and `revoke` removes it. V2 deleted it on disconnect. |
| 5 | Status chips say **"Needs you"** (LANGUAGE.md human label for `needs_attention`) where V2 showed the fixture `stateLabel` "Needs a human". | LANGUAGE.md owns the human labels; the fixture's `attentionReason` is still shown verbatim. |
| 6 | Heading sizes come only from `--type-h1`, `--type-lead`, `--type-body` plus weight and spacing. | V2's DESIGN §9 says so; V2's source used `calc()` multiples of `--type-h1`, which breaks that rule. |
| 7 | Discover can name **every** not-yet-connected fixture service, so stale, unknown and unavailable can each be met through Connect. | The proof path needs stale and unknown, not only the three V2 offered. |
| 8 | The receipt is per **fact**, with a fact list above it, not only the first fact. | V2 showed `facts[0]` only. |

### Inputs honoured
`docs/vnext/full-design-brief.md`; V2 and Concept 5; Field Guide docs; house tokens and `LANGUAGE.md`; the Connect contract `docs/vnext/connect/connect-contract.md`; `concept-brief-v2.md` requirements 1-6 and `concept-brief-v3.md`; the fixture. Rylee's standing preferences: dark by default with soft contrast, reduced motion respected, full keyboard support, 44px targets, plain words.

## 3. Information architecture

Worlds is the **Experience** layer: a costume over the stable skeleton. Platform contracts (the Connect contract) and Capabilities sit beneath it and do not change when the costume does ("Stable meaning. Mutable form."). The skeleton's four doors stay recognisable; Worlds gives each a counter-and-Dot form.

| Worlds destination (nav label) | Field Guide door | What lives here | Why it sits there |
|---|---|---|---|
| **Home** ("Your counter") | **Home**: orientation, what matters now | Status line, service slips, selected service, its facts and receipt, its app frame. The welcome lives here until done or skipped. | Home answers "what matters right now?" The counter is the Worlds form of that. |
| **Connect** | **Explore**: looking around, trying, synthesising | Discover, reference, safe test, preview, choose, publish. | Connecting is exploring what a service offers before committing. See open question Q1: the Connect *contract* is Platform/Capability; only this screen is Experience. |
| **Needs you** | **Needs You**: consequential human decisions | Services that need a person, proposals (inert), and the handoff to Project Home. | Authority stays with the human via Project Home's boundary. Worlds shows and links; it never decides. |
| **Memory** (page title "Agent facts") | **Guide**: understanding and continuity | Every shared fact from every connected service, with source, freshness, who can see, who can act, and scope. | This is where the person (and an agent) can trace any fact to its owner. It is the Guide's "leave a followable path". |
| **Settings** | not a door; a utility reachable from all four | Connected services, credential references, providers, live feed, motion and appearance, about. | Settings is about the person and the install, not a job in the skeleton. It sits apart in the rail and is the fifth phone tab. |

Rules:
- **Skeleton words stay out of the way.** Nav labels are Worlds words and plain words. The mapping above is for the team and the Field Guide link in About; it is not shown as jargon.
- **Dropping the costume must not break the product.** Every destination works with Dot, the stamp and the parcel removed.
- **A service is not a destination.** A service's surface is a *view* inside Home (and linked from Needs you and Memory). It is opened by choosing its slip; it keeps Worlds navigation around it.
- **Order of nav:** Home, Connect, Needs you, Memory, then Settings (separated at the rail's foot; last tab on phone). "Needs you" shows a count badge with its number in text (for example "Needs you, 1").

### Deep links (prototype routes, for back button and focus)
`#/welcome/1-3`, `#/home`, `#/home/{service-id}`, `#/home/{service-id}/{app}` , `#/connect/{step}`, `#/needs-you`, `#/memory`, `#/settings/{section}`. Browser Back moves one screen; focus returns to the control that opened the screen.

## 4. Every screen and state

Conventions in every table: **Carries** names which protected-feeling items (§1) the screen keeps. State words use the closed vocabulary mapped in LANGUAGE.md: Healthy, Needs you, Stale, Unavailable, Unknown, Not configured (and Working, Done, Failed for the connect test). A state always has a word; colour, glyph and Dot are supplemental.

### 4.0 Screen inventory

| ID | Screen | Route | States covered |
|---|---|---|---|
| W1-W3 | Welcome, three beats | `#/welcome/n` | default, skipped |
| H0 | Empty counter | `#/home` | first run, after skip, after last disconnect |
| H1 | Counter, composed | `#/home` | quiet, needs-you, stale, unknown, unavailable, offline |
| C1 | Connect: Discover | `#/connect/discover` | empty list, candidates, already-connected |
| C2 | Connect: Reference | `#/connect/reference` | pristine, dirty, valid, refused-secret, refused-own-secret, pending |
| C3 | Connect: Test | `#/connect/test` | idle, running, passed, failed, retry n of 3, limit |
| C4 | Connect: Preview | `#/connect/preview` | loading, facts, empty |
| C5 | Connect: Choose | `#/connect/choose` | all, some, last-one-locked |
| C6 | Connect: Publish and Connected | `#/connect/publish` | confirm, delivering, done |
| S1 | Service surface | `#/home/{id}` | facts, app, agent view (phone tabs) |
| S2 | App frame | within S1 | loading, loaded, refused, timeout |
| N1 | Needs you | `#/needs-you` | items, proposals, empty |
| N2 | Approval handoff | sheet or panel | not available (default), link ready (future) |
| D1 | Disconnect | dialog or sheet | confirm, working, done |
| M1 | Memory (Agent facts) | `#/memory` | rows, empty, filtered |
| T1-T5 | Settings sections | `#/settings/{section}` | each with loading, empty, error |
| X1 | Offline, error, loading | overlays | offline strip, failed to load, saving error |

### 4.1 Welcome (W1-W3) and the first run

First run opens on Home with the welcome card in the main area; the shell (top bar, nav) is already there, so nothing is hidden and "skip" is genuinely a skip. Three beats, one idea each. `#skip-welcome` is on **every** beat and goes straight to H0.

| Beat | What it shows | Primary action | Carries |
|---|---|---|---|
| **W1 Meet Dot** | Large Dot (awake, waving no more than a nod) in a sunken well beside the heading and two plain sentences about what Worlds is: a counter where services leave their facts and Worlds keeps them honest. | "Next" | 6, 12 |
| **W2 How one fact travels** | A static, fixture-true example: Media Shelf, "Items in library: 1,284", drawn as the three stations Source (Media Shelf, local), Worlds (shared as "Items in library"), Agent / Human (Agent can see it, a person can see it). One sentence: every fact says where it came from and who can act. No animation required. | "Next" | 5, 10 |
| **W3 Connect your first service** | Dot asleep-then-awake holding a slip; a short note that connecting takes six steps, shares only what the person picks, and never stores a secret. | "Connect a service" (goes to C1), and "Skip welcome" | 6, 7, 12 |

Rules: no countdowns, no auto-advance; "Back" from W2 and W3; the welcome is never shown again after finish or skip (stored in the mock session; **Settings > About > Replay the welcome** brings it back). Reduced motion: no Dot movement at all.

### 4.2 Empty counter (H0)

Shown after skip, and again whenever the last service is disconnected. This is "back to empty".

- Dot **asleep** in a dashed-outline well; heading "The counter is quiet"; one sentence; `#add-service` (also in the top bar).
- Status line: "No services are connected yet."
- Selected-service strip: "No service selected." with `#disconnect` visible and **disabled**, with the reason "Connect a service before disconnecting" as visible text, and `aria-describedby` pointing to it.
- Local Weather is **not** shown as a slip. It has no facts and is not connected; it appears in Settings > Providers as Not configured.
- Memory shows its own empty state; Settings shows empty lists (same sentence).

**Carries:** 1, 2, 6 (asleep), 9.

### 4.3 Home: the counter, composed (H1)

**Initial settled set:** the welcome-connected Media Shelf (Healthy) and Front Door (Needs you) (`media-shelf`, `front-door`, as V2). Front Door is selected on load, so `#disconnect` reads "Disconnect Front Door" on the first screen after the welcome.

Order (reading order and DOM order are the same):
1. Page header: "Your counter".
2. **Status line:** "1 needs you · checked 24s ago", with the live and simulate controls (see §5 for where they sit per width).
3. **Slips** (one per connected service). Each: name; kind; state chip (glyph + word); one-line reason (fixture `attentionReason` or `summary`); one peeked fact (label and value, or "No facts"); "Checked 24s ago". Selected slip shows the word "Selected" and a check shape plus the mint border (never colour alone).
4. **Service detail**: header (name, state chip, owner app, `#disconnect`), **fact list**, the **receipt** for the selected fact, the **Agent sees** line, then the **app frame** (secondary).
5. Dot at rest beside the freshness line, and "Updated just now" with a nod when a state word changes.

State presentation on a slip and in the detail:

| State | Chip | Slip reason line | Value shown |
|---|---|---|---|
| Healthy | check + "Healthy" | fixture `summary` | the value |
| Needs you | alert triangle + "Needs you" | fixture `attentionReason` first, then `summary` | the value |
| Stale | clock + "Stale" | "Nothing has changed since {time} that Worlds knows of." | last known value, labelled "last known" with its checked time |
| Unavailable | x in circle + "Unavailable" | fixture `summary` | "Unavailable" for a fact with no value; last good value kept and labelled |
| Unknown | question mark + "Unknown" | "Worlds has no current reading for this and won't guess." plus fixture `unknownReason` | "Unknown" (never blank, never 0) |
| Not configured | dash + "Not configured" | fixture `summary` | none; no placeholder |

**Quiet.** When the attention count is 0 the status line reads "Nothing needs you right now." Healthy and current are said in words, with no celebration. After `#simulate-change` with nothing else outstanding: "That was the last one. Nothing needs you right now." **Carries:** 1-5, 9, 10.

### 4.4 The Connect lifecycle (C1-C6)

Connect follows the contract's verbs in the order the person meets them: **Discover → Reference → Test → Preview → Choose → Publish**, then the **Connected** moment. It is one page with a step list (vertical on desktop, a "Step 3 of 6 · Test" line on phone). Every step has Back; Cancel returns to Home without changes; nothing is saved until Publish (discover and test write nothing; the contract's `register` and `publish` are what persist). No real network is called in the mock, and every step that would call one says so.

**Proof-path services.** Discover can list: Notes Sync (unavailable, the default first stop), Nightly Backup (stale), Garden Camera (unknown), Studio Printer (unavailable), plus anything the person disconnected. Local Weather is not offered: it is Not configured and belongs to Settings > Providers. The welcome-connected two (Media Shelf, Front Door) are listed under "Already at the counter".

| Step | What the person does | What Worlds says it will and won't do | Carries |
|---|---|---|---|
| **C1 Discover** | Picks a service from the candidates (name, kind, owner app, state word, one-line summary). | "Looking only. This sends no credential and changes nothing." (the contract's `discover` is a read-only probe that writes nothing). Candidates are proposals, not facts: each shows who says it exists. | 5 |
| **C2 Reference** | Enters a **credential reference**, not a secret, and ticks which parts of the service the reference may be used for (the **scope**, default all discovered facts, at least one). | The reference is checked for shape when the person leaves the field. Worlds shows the scope sentence *before* any test: "Worlds will send this reference only to: {list}." Next stays disabled until valid and scoped. | 11 |
| **C3 Test** | Presses "Run safe test". | One read, nothing changed on the owning service. The reference is sent only within scope. Static progress ("Testing…", no spinner). Passes first time by default; a labelled mock checkbox "Simulate a failed test (mock)" makes it fail. | 6 (static Dot, awake) |
| **C4 Preview** | Reads what the service offers. | Facts appear with their own states and are never merged: for Notes Sync, "Provider reachable: Unavailable", "Local notes waiting to upload: 3 notes, Live", "Last successful sync: Stale". | 5 |
| **C5 Choose** | Ticks the facts to share. | At least one must stay ticked. Plain sentence: what is shared with Worlds, the people using it, and agents; anything unticked is kept out and named later as "kept out". | 11 |
| **C6 Publish** | Presses "Publish". | Shows the summary, then Dot carries the parcel (400ms desktop, 300ms phone), then the **Connected** stamp lands and the slip appears. Offers "Open {service}", "Add one more", and "Manage this reference in Settings". | 7, 8 |

**C2 Reference field states** (pristine, dirty, valid, refused, pending):
- **Pristine:** "Not edited yet. Use the form ref://name."
- **Dirty:** "Edited. Worlds checks it when you leave the field."
- **Valid:** a check shape and "Looks like a reference."
- **Refused (looks like a secret):** anything not shaped like a reference is refused. The field is **cleared at once**, the secret is never put into any message, announcement or stored state, and the message says nothing was saved. This answers the contract's "no secret value round-trip into the browser".
- **Refused (Worlds' own secret):** a reference to one of Worlds' own secrets (names beginning `PW_`, `OIDC_` or containing `BOOTSTRAP`) is refused with its own sentence (contract §3, endpoint 6, and threat "borrowing a Worlds secret").
- **Pending:** "Saving reference…" static, field read-only.

**C3 Test states**: idle, running (static), **passed** (what was read, how long it took, "Nothing on {service} was changed"), **failed** (specific reason from the fixture `lastError`, for example "provider timed out"; the reference stays in the field; "last tried {n}s ago"; "try {n} of 3"; "Try again"; recovery text), and **limit reached** after three tries ("Run safe test" disabled until the reference is edited). The failure never blames the person.

**C4 Preview states**: loading (static Dot, "Dot is fetching the facts…", with a link to the owner fallback if it stalls), facts, and empty ("This service offers no facts yet. Nothing is guessed.").

**C6 outcomes.** Optimistic: the new slip is in the counter behind the Connected card the moment Publish is pressed; the parcel and stamp settle it. Afterwards the service's receipt says, in its Worlds station, "Connected {when} · reference held, never the secret." This line is the welcome's promise made concrete and becomes an ordinary row for every later connection.

**Leaving the flow:** Cancel, browser Back and Escape on the step page all ask nothing when nothing was saved; the entered reference is dropped.

### 4.5 A service's surface (S1)

Inside Home, with Worlds navigation, the MOCKUP label and a back path visible around it. On desktop it is the right-hand detail pane; on phone it is a pushed screen.

Order: **header** (name, kind, state chip, "Owned by Home app", freshness, `#disconnect`) → **fact list** (compact rows; choose one) → **receipt** (the hero) → **Agent sees** line → **app frame** (secondary strip) → **kept out** note if the person left facts out in Choose ("1 fact is kept out of Worlds. You chose not to share it.").

**The receipt (daily signature).** Three stations in a row (stacked on phone):
- **Source:** who produced it ("Home app reported **unlocked**"), source word (local, external or none), state word, "Checked 24s ago".
- **Worlds:** "Shared as Back door." The fact id, freshness word, and the provenance row ("Connected during first run · reference held, never the secret", or "Connected {when}"). Also the reference's **scope** word: "Scoped to 3 facts" or "Unscoped" (see §10).
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

Below, **Proposals** for that service (§4.8), always inert.

**N2 the handoff panel (fail-closed by default).** Opening "Ask Project Home to review" shows, in a panel (sheet on phone):
- Heading: "Approvals aren't connected yet."
- Body: "Worlds can't send this to Project Home yet, so nothing was sent. You can decide in the Home app instead."
- Buttons: "Open Home app" (primary), "Close".
- A line stating what would happen when it is connected: "When approvals are connected, this will open Project Home with the proposal. Project Home, not Worlds, makes the decision." (so the person knows the intended path without it appearing to work).
- Never shown: "Sent", "Pending approval", "Approved", a spinner, or a recorded-handoff receipt.

Future-ready state **"Link ready"** (not reachable in this mock): the panel says what will be opened, to which system, and that Worlds will not record a decision; the button is a real link to Project Home. Any other state, including "unknown whether approvals are connected", renders as the fail-closed state. Fail-closed is the default; "connected" must be proven, not assumed (Sol condition 1).

**N1 empty:** Dot rests; "Nothing needs you right now."

### 4.8 Proposals shown as inert

A **proposal** is a consequential action the owner might one day make callable (contract `propose-action`, `exposed: false`). Worlds shows them so the person can read what is being proposed; it never runs them.

- Appears in Needs you (under its service) and in the service surface under "Proposed actions".
- Each proposal: label ("Lock the back door"); service; the effect word "Changes the real world"; "Proposed. Not active."; and **who would decide** ("Decided by Home" or "Decided in Project Home").
- **No Run, Do it or Approve button exists on a proposal in Worlds.** The only controls are "Open Home app" and "Ask Project Home to review" (§4.7). A proposal row is not focusable as a command; it is read as text.
- Fixture proposals used: "Lock the back door" (Front Door, `lock-back-door`) and "Check provider now" (Notes Sync, `probe-provider`, attempts 2, last error "provider timed out"). For Notes Sync the retry is an owner handoff to the Memory app, never an approval.
- No agent can propose, adopt or run anything; the Agent line on a proposal says "Agent can't do this."

### 4.9 Stale, unknown, unavailable, revoked, not configured

These are **states of the same screens**, not separate pages. Each keeps the last-known value visible and labelled, says when Worlds last checked, offers a route to the owner, and never promises recovery.

| State | Cause (mock) | What the person sees | What they can do |
|---|---|---|---|
| **Stale** | Scripted: Media Shelf misses a re-check (once per session). Real rule is UNKNOWN (Q6). | Chip "Stale (simulated)". Last value labelled "last known". "Nothing has changed since 2:23 PM that Worlds knows of." Receipt Source station: "Last checked 3m ago." | "Check now" (restores Healthy in the mock), "Open Media app". |
| **Unknown** | Fixture: Garden Camera. Also scripted: if Media Shelf stays stale for 60 more seconds without "Check now", it becomes Unknown (simulated). | "Worlds has no current reading for this and won't guess." plus fixture `unknownReason`. Value "Unknown" (never blank). Last known value, if any, shown in its own labelled line, not as the current value. Agent can't assert (fixture reason for Garden Camera "Human detected today"). | "Open {owner app}". No retry is invented. |
| **Unavailable** | Fixture: Notes Sync, Studio Printer; or a service that drops off. | Chip "Unavailable". "That service stopped answering. Its last facts are still here, marked with when Worlds checked." Fixture last error. | "Check provider now" (owner handoff to Memory app), "Open Memory app". |
| **Revoked credential** | The provider stopped accepting the reference (mock trigger in About). | Chip "Unavailable", reason "Notes Sync stopped accepting this reference." Last facts kept and labelled stale. The error is classed, not free text from the provider. | "Replace reference" (opens Connect at Reference, field empty), "Open Memory app". Worlds cannot fix the provider's side. |
| **Not configured** | The person removed the reference in Settings, or Local Weather. | Chip "Not configured"; fixture summary; no facts, no placeholder. | "Reconnect" (Connect step 2, empty field) or "Open provider settings". |

Rule: Stale and "waiting" are separate. A reassuring state never hides a stale one, and an unknown or unclassified case is shown as Unknown, never as fine.

### 4.10 Disconnect (D1) and back to empty

`#disconnect` is always visible for the selected service and opens a confirm that **names the service and says what goes and what stays**. It is a dialog on desktop and tablet and a bottom sheet on phone. It is a consequential surface: no Dot, no flourish.

| Goes (from Worlds) | Stays |
|---|---|
| The slip, its facts, its receipts, its app frame, and the kept-out note. | The owning app, untouched. Nothing is sent to it and nothing in it changes. |
| What Worlds has remembered of the last-known values. | The credential reference (held, nothing is sent with it) unless the person ticks "Also remove the reference". Worlds can't revoke the service's own key; if the reference is removed, the dialog says to rotate the key with the owning app. |
| | Anything awaiting approval is **listed, not cancelled**: "1 action is waiting for approval in Project Home. Disconnecting doesn't cancel it." |

Buttons: **Disconnect** (danger) and **Keep service**. Escape = Keep service. Focus: opens on the heading; Keep service is the first button (a safe default); on close, focus returns to `#disconnect` or, after a successful disconnect, to the next slip (or to `#add-service` when none is left).

**Optimistic then settled:** the slip, facts and frame leave in the same frame the confirm is pressed; a status message then names exactly what went ("Notes Sync's 3 facts and its app frame are removed from Worlds. The Memory app is untouched. Its reference is kept.") and offers "Reconnect" (Connect with the service and reference prefilled). If it was the last service: H0 (empty counter), Dot falls asleep (200ms, none under reduced motion). **Worlds still works:** Memory and Settings show their empty states; Connect still starts.

### 4.11 Memory: Agent facts (M1)

One list of every fact from every connected service, using the **same fact-row component** as S1, so one fact reads the same everywhere.

Each row: label, value (or Unknown / Unavailable), state word, service name, source (local, external or none), "observed {time}", **Agent can** and **Human can** as two labelled lines, the fixture reason where there is one, and the **scope word** for the service's reference ("Scoped" or "Unscoped"). A `none` permission renders as a named, focusable, disabled control with its reason. Filter: "All services" or one service (a select, keyboard reachable). Rows group under service headings so a screen-reader user can jump by heading.

Empty: "No agent facts yet. Connect a service to add facts without guessing." with `#add-service`.

**Who sees, who acts** words: a person reads "Agent can: see it" and "Human can: act on it in the Home app". Values and permissions change only by an event the owner caused (§9), never by Worlds on its own.

### 4.12 Settings (T1-T5)

Desktop and tablet: a section sub-list beside the pane. Phone: one scrolling column of section headings, tab bar visible. Sections, in this order:

1. **Connected services:** one row per service: name, state word, freshness, its own `#disconnect-{service-id}` (same confirm). Empty: "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you."
2. **Credential references:** heading line "Worlds stores a credential reference, never the secret." Each row: service, the reference text (wraps, never truncated), **scope** ("Scoped to: 3 facts" or **"Unscoped (older connection): this reference can be used for anything this service offers"**), and Replace, Remove, and for unscoped ones "Narrow scope". Media Shelf and Front Door show UI-only handles `ref://media-shelf` and `ref://front-door`, labelled as mock handles, and are **Unscoped** (to show the legacy label, Sol condition 3); Notes Sync, connected through Connect, is **Scoped**. Remove asks first, then makes the service Not configured and offers Reconnect; it doesn't disconnect. Replace is an inline field with the same checks and secret refusal. A kept reference from a disconnected service appears as "Held, not in use". Empty: "No references stored. Connecting a service will add one."
3. **Providers:** Local Weather, Not configured. Fixture summary and `emptyReason` shown; no facts; "Open provider settings" → `app://settings/providers`.
4. **Live feed:** `#settings-live` switch (the same state as `#live-toggle`; the two always agree); check cadence 5s (default), 10s, 30s; **Motion:** Follow system (default) / Reduce always; **Theme:** Dark (default), Light, High contrast, Low stimulation, Follow system; **Text size** note ("Worlds follows your browser's text size; zoom to 200% reflows"). Line: "Reduced motion removes animation, not data."
5. **About:** MOCKUP, "Synthetic data only", how Worlds fits the Field Guide (one sentence and link), the UNKNOWNs (§12), "Replay the welcome", a disabled named "Keep history after disconnect" with reason "Not decided yet; this mockup keeps nothing", and **Mock controls** for reviewers: "Make Media Shelf stale now", "Pretend Notes Sync refused its reference", "Show Needs you approvals as connected" (this last one only shows what the future state would look like and is labelled as a preview; the default is off).

States per section: loading (static Dot, "Dot is fetching your settings…"), empty (as above), error ("Couldn't save that. Nothing was changed. Try again."), long references wrap.

### 4.13 Offline, error, loading (X1)

- **Offline** (the browser has no connection): a strip under the top bar, "You're offline. Worlds is showing what it last knew, marked with when." The live feed pauses (`#live-toggle` reads "Paused · offline"), ages stop growing (they would be false), and states are shown as Stale-with-time. Returns automatically: "Back online. Checking now."
- **Couldn't load a screen:** "Worlds couldn't load this. Your services and references weren't changed." "Try again" and "Go to Home". Dot rests; no blame.
- **Save failed:** "Couldn't save that. Nothing was changed. Try again."
- **Loading:** always static (a still Dot and a sentence). No spinners, no shimmer.

## 5. Layouts

Three layouts, designed separately. Breakpoints in rem so they follow text size: **desktop ≥ 60rem** (designed at 90rem, which is 1440px); **tablet 48rem to under 60rem** (designed at 48rem, 768px); **phone under 48rem** (designed at 24.375rem, 390px). DOM order equals reading order at every width; the layout changes by CSS, not by reordering content. No sideways scroll; at 200% zoom content reflows and nothing clips.

### 5.1 Desktop (1440)

- **Top bar** (sticky): MOCKUP label and "Worlds"; then `#live-toggle` ("Live · checked 24s ago" / "Paused · frozen at 24s"), `#simulate-change`, `#add-service`, `#disconnect` ("Disconnect Front Door", or disabled "Disconnect" when none).
- **Rail** (left, `--surface-sunken`): Home, Connect, Needs you (badge), Memory; Settings at the foot.
- **Home:** two columns: slips (a column of roughly 23rem) and the detail pane. Pane order: header, fact list, receipt (hero), Agent sees, app frame.
- **Connect:** vertical step list at the left of the page, step body in the middle, a summary of what will happen at the right if width allows; Back/Next bar at the foot of the body.
- **Needs you and Memory:** single reading column up to a comfortable measure (about 55ch for prose), cards full width.
- **Settings:** a 14rem section list and the pane.
- **Dialogs:** centred, `--surface-raised`, scrim `--surface-sunken`.

### 5.2 Tablet (768)

- Top bar wraps to two rows (title and live controls, then add and disconnect). The rail collapses to a narrow strip of icons; the **label shows on focus and hover** and the accessible name is always the label.
- **Home:** slips become a two-column grid above the detail pane; the pane is full width; receipt stations stay three across (they fit at 768) and drop to a column under 40rem.
- **Connect:** step list becomes a horizontal numbered strip above the body.
- **Settings:** the section list becomes a horizontal scroll-free wrapped row of links above the pane.
- Targets stay 44px; gaps do not collapse.

### 5.3 Phone (390)

Own navigation and reading order, not a squeezed desktop.

- **Page header:** MOCKUP label and title.
- **Status row:** the one-line status ("1 needs you · checked 24s ago") then `#live-toggle` and `#simulate-change` as two 44px-high buttons (wrap to two lines if needed).
- **Selected-service card:** state chip, reason, "Open Front Door" and `#disconnect` ("Disconnect Front Door") on the first screen. With none selected: the honest line plus disabled `#disconnect` with its reason.
- **Slips:** full width, each at least 44px, state word and freshness visible.
- **`#add-service`:** a floating 44px-high pill at right 1rem, bottom `calc(<tab bar height> + env(safe-area-inset-bottom) + 1rem)`; the list has enough bottom padding so it never covers a slip. Hidden on pushed screens and in Connect.
- **Bottom tab bar:** Home, Connect, Needs you (with count in text), Memory, Settings. Five equal tabs, each at least 44px tall and wide (the bar is 3.5rem plus safe area). The tab bar is `aria-label="Worlds sections"`.

**Exactly one bottom strip at a time:**

| Screen | Bottom strip |
|---|---|
| Home, Needs you, Memory, Settings | Tab bar |
| Pushed service screen | none; Back is in the top bar |
| Connect | the sticky Back / Next bar (replaces the tab bar) |
| Dialogs (disconnect, handoff) | bottom sheet over the tab bar, scrim behind |
| Offline strip | top, under the page header (never a second bottom strip) |

**Pushed service screen:** Back (top bar), the status row, then `#disconnect` under the heading, then tabs **Facts / App / Agent view**; Facts first; receipt first inside Facts. Focus moves to the new heading on every push and returns to the invoking slip on Back.

## 6. Components and tokens

Tokens are the house names in `docs/vnext/house/tokens.css`. **No colour, gap, radius or text size is written as a literal outside tokens.** Lengths that have no token (target floor, hairlines, panel widths, durations) are Worlds-local handles with a `--wv-` prefix, defined once, in `rem`; they hold no colour. (The tokens document is explicit that the target floor is Play-Nice's and has no token; Q8 asks whether these handles should be promoted.)

**Local handles (layout, not colour):** `--wv-target-min` (the 44px floor, as 2.75rem), `--wv-hairline`, `--wv-frame-line`, `--wv-ring-width`, `--wv-rail-w`, `--wv-slips-w`, `--wv-ease` (`cubic-bezier(.2,0,0,1)`), `--wv-dur-state` (150ms; 120ms phone), `--wv-dur-stagger` (60ms), `--wv-dur-sheet` (200ms), `--wv-dur-nod` (200ms), `--wv-dur-carry` (400ms; 300ms phone). Reduced motion sets durations to 0 and travel to 0.

| Component | Tokens it uses | States |
|---|---|---|
| **App shell** (top bar, rail, tab bar, main) | `--surface-base` page, `--surface-raised` top bar and tab bar, `--surface-sunken` rail, `--border-line` edges, `--space-*` | wide, tablet, phone; offline strip |
| **MOCKUP label** | `--accent-mint` text and border, `--radius-sm`, `--type-micro` | always visible on every screen |
| **Status line** | `--text-soft`, `--status-needs-you` for the count word, `--type-body` | quiet, needs you, paused, offline |
| **Live toggle / Simulate** | button tokens; `aria-pressed` on the toggle | on, paused, offline; focus, hover, active, disabled |
| **Button** (primary, secondary, danger, text, icon) | primary: `--action-primary`, `--action-on-primary`; secondary: `--surface-raised`, `--border-line-strong`, `--text-primary`; danger: `--status-failed` border and text; `--radius-full`; `--type-label` | default, hover, focus-visible (`--focus-ring`), active, disabled, pressed; at least `--wv-target-min` tall |
| **State chip** | `--status-*` text on `--surface-raised` or `--surface-sunken`, a glyph, the word, `--type-small` | healthy, needs you, stale, unavailable, unknown, not configured; never glyph or colour alone |
| **Slip** | `--surface-raised`, `--radius-lg`, `--border-line`, selected `--action-primary` border and the word "Selected" | default, hover (lifts), selected, focus, with stale/unknown/unavailable content |
| **Service header** | `--text-primary`, Young Serif, `--type-h1` | per state |
| **Fact row** | `--surface-raised`, `--border-line`, `--type-lead` for the value | live, stale, unavailable, unknown; permission none |
| **Receipt** | frame `--border-line-strong`, `--radius-lg`, `--surface-raised`; stations `--surface-sunken`, `--radius-md` | idle, updating (ticks settle), reduced-motion (all ticked at once) |
| **Agent can / Human can lines** | `--text-primary`, `--text-soft` for reasons | read, act, write, none (disabled and named) |
| **Provenance row** | `--text-soft`, `--type-small` | welcome, connected later, unscoped, scoped |
| **App frame** | `--surface-raised`, `--border-line`, inner `--surface-sunken` | loading, loaded, refused, timeout |
| **Proposal row** | `--surface-sunken`, dashed `--border-line-strong` | proposed (inert); never active in this mock |
| **Handoff panel** | `--surface-raised`, `--border-line-strong` | not available (default), link ready (future) |
| **Connect stepper** | progress uses `--action-primary` (done) and `--border-line` (to do); current step has a numeral, a word and `aria-current="step"` | pending, current, done, error |
| **Reference field** | `--surface-sunken`, `--border-line-strong`, error `--status-failed`, valid `--status-done`, each with a word and glyph | pristine, dirty, valid, refused, pending, disabled |
| **Scope list** | checkboxes on `--surface-sunken` rows | all, some, none (invalid) |
| **Test panel** | `--surface-sunken` | idle, running, passed, failed, retry n of 3, limit |
| **Fact chooser** | checkbox rows | all, some, last-one-locked (`aria-disabled`, with its reason) |
| **Dot** | shell `--accent-apricot`, body `--accent-mint`, ink `--surface-base`, parcel paper `--surface-raised` with `--border-line-strong` | asleep, awake, nod, parcel, resting; `aria-hidden` except where a sentence names Dot |
| **Stamp** | `--status-done` border and text, Young Serif capitals, tilted -4° | landing (200ms), static (reduced motion: no travel) |
| **Dialog / sheet** | panel `--surface-raised`, scrim `--surface-sunken`, `--radius-lg` | open, working, closed; Escape closes |
| **Toast / live line** | `--surface-raised`, `--status-done` border for success, one `role="status"` | hidden, visible (3.6s, persistent while paused) |
| **Empty state** | dashed `--border-line-strong`, `--text-soft` | first run, after disconnect, empty list |
| **Settings section, switch, select, radios** | `--surface-raised`, `--border-line-strong` | each with loading, empty, error |
| **Banner** (offline, error) | `--surface-raised`, `--status-stale` or `--status-failed` edge with a word | offline, back online, load error |

**Type.** Display: Young Serif (400); body: Atkinson Hyperlegible Next (400, 600 for labels, 700 for state words); system fallbacks (Georgia, Verdana) and **no external fonts in the prototype**. Roles: page and service titles `--type-h1`; slip and card titles `--type-lead`; body `--type-body`; labels and chips `--type-label`; captions `--type-small`; MOCKUP label `--type-micro`. The welcome heading uses `--type-h1` with more space and weight, not a bigger literal size.

**Icons.** Status glyphs are a single simple line set (check, alert triangle, clock, x in circle, question mark, dash), always beside their word. The house painted sticker icons (`ui-planet`, `ui-star`, `ui-gear`, and so on) may be used for nav in production at 20px or more beside their label; the prototype uses plain inline line glyphs as stand-ins. No emoji, no gradient.

## 7. Copy

Rule: machine truth, then plain words, then (sometimes) a warm line. Warmth never creates a state, a deadline or certainty. Short sentences; say what happened, what it means, and what to do. Avoid "provider", "capability", "endpoint" and "scope" in headings; gloss them when they must appear.

### 7.1 Status words and short labels
Healthy · Needs you · Stale · Unavailable · Unknown · Not configured · Working · Done · Failed. Freshness: "Checked 24s ago", "3m ago", "2h ago", "Never checked". Source: "From this device", "From the service's provider", "No source answered". Permissions: **Agent can:** see it · act on it · edit the local copy · not at all. **Human can:** see it · act on it in the {app} · edit the local copy · not at all.

### 7.2 Strings by screen

| Where | String |
|---|---|
| Top bar | "MOCKUP" · "Live · checked 24s ago" · "Paused · frozen at 24s" · "Paused · offline" · "Simulate change" (full name for assistive tech: "Simulate the Home app reporting the back door locked") · "Add service" · "Disconnect Front Door" · disabled: "Disconnect" with "Connect a service before disconnecting" |
| W1 | "Hello, I'm Dot." / "Worlds is a small counter. Your services leave their facts here, and Worlds keeps them honest." / "Next" / "Skip welcome" |
| W2 | "Here's one fact on its way." / "Media Shelf says it holds 1,284 items. Worlds shows where that came from and who can see it or act on it." / "Every fact says where it came from and who can act on it." |
| W3 | "Let's connect your first service." / "It takes six short steps. You choose what to share. Worlds never keeps a secret." / "Connect a service" / "Skip welcome" |
| H0 | "The counter is quiet." / "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you." / "No services are connected yet." |
| H1 status | "1 needs you · checked 24s ago" · "Nothing needs you right now." · "That was the last one. Nothing needs you right now." |
| Receipt | "Source" · "Worlds" · "Agent / Human" · "Shared as {label}." · "Connected during first run · reference held, never the secret." · "Connected {time} · reference held, never the secret." · "Scoped to {n} facts" · "Unscoped" · "Updated just now" |
| Kept out | "{n} fact is kept out of Worlds. You chose not to share it." |
| Stale | "Nothing has changed since {time} that Worlds knows of." · button "Check now" |
| Unknown | "Worlds has no current reading for this and won't guess." |
| Unavailable | "That service stopped answering. Its last facts are still here, marked with when Worlds checked." |
| Revoked | "{Name} stopped accepting this reference. Its last facts are still here, marked with when Worlds checked. Worlds can't fix this on {Name}'s side." · "Replace reference" |
| Not configured | "{Name} isn't set up. Connect it, or open its settings." · "Reconnect" |
| Frame | "This app didn't load inside Worlds." · "Use the link below, or try again." · "Try again" · "Open Home app" · "This app doesn't receive Worlds' credentials." |
| Needs you | "Needs you" · "A decision is required; the local app will not act on its own." · "Open Home app" · "Ask Project Home to review" · "Not available yet" · "Nothing needs you right now." |
| Handoff | "Approvals aren't connected yet." · "Worlds can't send this to Project Home yet, so nothing was sent. You can decide in the Home app instead." · "When approvals are connected, this will open Project Home with the proposal. Project Home, not Worlds, makes the decision." |
| Proposal | "Proposed. Not active." · "Worlds can't run this. It can only show it." · "Agent can't do this." · "Decided in the Home app." · "Decided in Project Home." |
| Simulate | "The Home app reported the back door locked. Worlds locked nothing." |
| C1 | "Find a service" / "Looking only. This sends no credential and changes nothing." / "Already at the counter" / "No other services to connect right now." |
| C2 | "Credential reference" / "A reference is a name that points to a secret kept somewhere safe. Worlds stores the name, never the secret." / "Worlds will send this reference only to: {list}." / pristine "Not edited yet. Use the form ref://name." / dirty "Edited. Worlds checks it when you leave the field." / valid "Looks like a reference." |
| C2 refused | "That looks like a secret, not a reference. Nothing was saved and the field was cleared. Worlds stores a credential reference, never the secret. Use something like ref://notes-sync." |
| C2 own secret | "That reference points at one of Worlds' own secrets, so it can't be used for a service." |
| C3 | "Run safe test" / "Testing…" / "Passed. Worlds read {n} facts in {ms}ms. Nothing on {Name} was changed." / failed "That didn't go through, and the reference is still here. Try again whenever you like." / cause "{lastError}" / "last tried {n}s ago" / "try {n} of 3" / limit "That's three tries. Check the reference with the owning app, then edit it to start again." / mock "Simulate a failed test (mock)" |
| C4 | loading "Dot is fetching the facts…" / empty "This service offers no facts yet. Nothing is guessed." / "These are separate facts and stay separate." |
| C5 | "Choose what to share" / "Worlds, people who use it, and agents will see only what's ticked." / "At least one fact must stay shared." |
| C6 | "Publish" / "Dot is carrying the parcel to the counter…" / "Connected" / "{Name} is at the counter." / "Open {Name}" / "Add one more" / "Manage this reference in Settings" |
| D1 | title "Disconnect {Name}?" / "{Name}'s {n} facts and its app frame will be removed from Worlds. The {App} app is untouched." / "Its reference stays in Settings, held and unused." / checkbox "Also remove the reference" / if ticked "Worlds can't revoke the key itself. Rotate it with the {App} app." / "Disconnect" / "Keep service" / after: "{Name}'s {n} facts and its app frame are removed from Worlds. The {App} app is untouched. Its reference is kept." · "Reconnect" |
| Settings | "Worlds stores a credential reference, never the secret." / "No references stored. Connecting a service will add one." / "Unscoped (older connection): this reference can be used for anything this service offers." / "Narrow scope" / "Reduced motion removes animation, not data." / "Not decided yet; this mockup keeps nothing." / "Couldn't save that. Nothing was changed. Try again." |
| Offline | "You're offline. Worlds is showing what it last knew, marked with when." / "Back online. Checking now." / "Worlds couldn't load this. Your services and references weren't changed." |

### 7.3 Announcements (one polite live region)
Routine ticks are **not** announced. Batched, polite messages only for: published ("Connected Notes Sync. Its slip and facts are now at the counter."), disconnected (the D1 sentence), a state change ("Media Shelf is now Stale."), a failed test (once, assertive: "Test didn't pass."), live feed paused or resumed, and "Updated across Home, Front Door and Agent facts."

## 8. Motion and accessibility

### 8.1 Motion

One curve, `--wv-ease`. Motion confirms a change that has already happened.

| What moves | Why | Normal | Reduced motion |
|---|---|---|---|
| State word on a slip, chip and receipt | confirm a change | cross-fade 150ms (120ms phone) | instant swap, opacity only |
| Receipt stations | show the journey settling | tick at Source, Worlds, Agent / Human, 60ms apart | all ticked at once, no stagger |
| Dot nod | "I noticed" beside "Updated just now" | 200ms, once | no nod; "Updated just now" stays 3s |
| Dot's parcel | publishing | travels 400ms desktop, 300ms phone | parcel appears at the counter, no travel |
| Connected stamp | the proud moment | scales in and lands tilted, 200ms | appears in place |
| Slip hover and select | affordance | lifts about 2px; scale only on buttons, 1.03, off on touch | no transform |
| Dialog and sheet | context | 200ms | none |
| Step change in Connect | continuity | content fades 150ms | instant |
| Plain feed tick | freshness | only the age number changes | same |

Respect the OS reduced-motion setting unconditionally; Settings can additionally force "Reduce always". Reduced motion removes animation, never data. Nothing flashes. No parallax. Dot's idle pose is still.

### 8.2 Keyboard map
- **No single-key shortcuts** (avoids accidental triggers and conflicts). Everything is reached with Tab, Shift+Tab, Enter, Space, arrow keys inside radio groups and selects, and Escape.
- **Skip link** "Skip to Worlds" is first; it moves focus to `main`.
- **Tab order** equals reading order: skip link; top bar controls (`#live-toggle`, `#simulate-change`, `#add-service`, `#disconnect`); nav (rail or tab bar); main content in the order in §4; dialogs trap focus and return it.
- **Slips:** each is a button with `aria-pressed`; Enter or Space selects. **Fact rows:** buttons with `aria-pressed`. **Step list:** not focusable except the Back / Next buttons.
- **Escape** closes dialogs, sheets and the handoff panel and returns focus to the invoking control. Escape on Connect asks nothing when nothing is saved.
- Disabled-with-reason controls use `aria-disabled="true"` so they stay focusable and readable, never the `disabled` attribute that drops them from the tab order (permission `none`, disabled `#disconnect`, the last chosen fact, the history option).
- **Focus visible** everywhere: `--focus-ring`, thick, offset, from tokens in every theme (not a Worlds-specific teal). Focus is never hidden behind the top bar or the tab bar (scroll margin set).
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
- **Re-check (simulated):** healthy local services re-check on a schedule: Front Door every 20s (only once it is Healthy) and Media Shelf every 30s. Values do not change; `checkedAt` and `observedAt` do. Services that are Needs you, Unavailable, Unknown, Not configured or already Stale never re-check; their ages grow and their words stay.
- **Stale event:** once per session, 90s after first load, Media Shelf (only while Healthy with all facts live) misses a re-check and becomes "Stale (simulated)". "Check now" restores it. If it is left, 60s later it becomes Unknown (simulated), keeping its last value only as a labelled "last known". Pausing freezes both timers.
- **`#simulate-change`:** the Home app reports the back door locked (fixture `crossViewExample`, `lock-back-door`). In one frame: `frontdoor.back-door` reads "locked", Front Door becomes Healthy, the attention count goes 1 → 0 in the status line, the Needs you badge, the slip, the service header and Memory; its proposal (§4.8) stays shown as proposed and unchanged, because Worlds did not run it. The line "The Home app reported the back door locked. Worlds locked nothing." stays visible, with the owner link "Open Home app". The change comes **from the owning app**, not from Worlds.
- **Optimistic then settled:** Publish and Disconnect show their result in the same frame (the slip appears or disappears), then settle visibly (parcel and stamp; the removal message) and name what changed. A failed Publish removes the optimistic slip and says why.
- **Pause and resume:** `#live-toggle` (`aria-pressed`, on by default) freezes the clock, ticks, re-checks and the stale event; data, actions and `#simulate-change` still work. Resume continues from the frozen time. It reads "Live · checked 24s ago" or "Paused · frozen at 24s". `#settings-live` is the same state and always agrees.
- **Announcements:** routine ticks are silent; meaningful changes are batched into one polite message (§7.3).
- **Words for the feed:** every feed-driven change carries the word "simulated" where it appears, so nothing reads as a real reading.

## 10. Trust and boundaries

Sol's six conditions are pre-prototype gates (Project Home decision 145: "Approve the boundaries; Sol's six conditions become pre-prototype gates"). This design makes each visible. Items marked *gate check* are things the prototype must show before it is shown to anyone as proof.

| # | Condition | Where it appears in the UI | Gate check |
|---|---|---|---|
| 1 | **Project Home approval is fail-closed.** Never shown as working; an approval is a link or handoff that can say "not available yet". | §4.7 N1 and N2: "Ask Project Home to review" is labelled "Not available yet"; the panel says "Approvals aren't connected yet" and "nothing was sent". No "sent", "pending", "approved", spinner or recorded-handoff receipt anywhere. Unknown connectivity renders as not available. Settings > About > Mock controls can preview the future state but it is labelled a preview and off by default. | Click the handoff in the default build and confirm nothing reads as done. |
| 2 | **Proposed actions are inert.** Shown as proposals; never executable from Worlds. | §4.8: "Proposed. Not active." There is no Run, Do it or Approve control on a proposal. "Agent can't do this." `#simulate-change` models an *owner-originated* change, not execution. | Search every screen for a control that executes a proposal; there must be none. |
| 3 | **Scoped auth before any network call; legacy unscoped labelled "unscoped".** | §4.4 C2: the scope sentence "Worlds will send this reference only to: {list}" appears and must be accepted before Test can run; Discover says it "sends no credential". Settings > Credential references and the receipt's Worlds station show "Scoped to {n} facts" or "Unscoped", with "Narrow scope" for unscoped ones (Media Shelf, Front Door as mock legacy). | Test cannot run with no scope; Unscoped is visible in Settings, Memory and the receipt. |
| 4-5 | **Contract fixes** (completed in the Connect contract, draft PR #329). | Not a UI element. The UI follows the contract as it stands: six verbs, `disconnect` keeps the reference and `revoke` removes it, `external_state_untouched` is said plainly. | The spec and prototype cite only fields the contract has. |
| 6 | **The read-only path is proven first: keyboard, mobile, stale and unknown.** | Everything in this design before Needs you is read-only: connect, preview, publish, see, age, link out, disconnect. The acceptance path (below) is run first. | See "Read-only path acceptance". |

**Read-only path acceptance (condition 6), the proof path.** All of this must pass before proposals, approval or any later work is built on top of it:
1. Empty counter → welcome (skip works) → Discover → Reference (with a secret refused and cleared) → Test (a failure, then a pass) → Preview → Choose → Publish → Connected.
2. The new fact goes **Stale** (word, last-known value, time), then **Unknown** (words, no number).
3. Follow a link out to the owning app (Open Memory app).
4. **Disconnect** (what goes, what stays), then the counter is empty and **Worlds still works** (Memory, Settings, Connect).
5. All of it by **keyboard alone** (visible focus, focus returns, Escape closes), and on a **390px phone** (no sideways scroll, 44px targets, one bottom strip).
6. In all four themes, and with reduced motion on.

**Other boundaries kept visible**
- **Worlds owns meaning; providers own mechanics.** Worlds shows and links; it never mints authority. A governed action goes to the owning system (Project Home for approvals, the service's own app otherwise). "Worlds locked nothing." is said in words.
- **No secret ever.** Only references are accepted, and a refused secret is cleared and never echoed.
- **Disconnect and revoke are honest.** Disconnect keeps the reference; removing it says Worlds can't revoke the service's own key.
- **Nothing is shown as a real reading.** MOCKUP label on every screen; "simulated" on every feed-driven change; synthetic data only.
- **The frame gets no Worlds credentials.**

## 11. What the prototype must show (pointer to deliverable 2)

The prototype implements this spec as one self-contained HTML file with no network, using the house tokens only. Checker ids: `#skip-welcome` (every welcome beat), `#add-service`, `#disconnect` (visible on the first screen; confirm button labelled "Disconnect"), `#simulate-change`, `#live-toggle`, plus `#settings-live` and `#disconnect-{service-id}` in Settings. Visible MOCKUP label. Desktop, tablet and phone layouts. It must demonstrate the proof path in §10 and the failure path (a failed test, a refused secret, a refused frame, a fail-closed handoff).

## 12. Open questions and UNKNOWNs

**Questions for Rylee or the team**
- **Q1.** Does Connect truly belong at the Explore door? The Connect contract is Platform/Capability; only its screen is Experience. If Explore later means Hive Works exploration, Connect could instead be a Home action with no door of its own.
- **Q2.** Should the first run suggest exactly two services, offer two, or offer none (as W3 does now)? (Carried from V2.)
- **Q3.** Should disconnect keep the reference by default (as the contract does) or remove it? This design keeps it, with a tick to remove it.
- **Q5.** Status label: this design uses LANGUAGE.md's "Needs you" where V2 showed the fixture's "Needs a human". Confirm.
- **Q7.** "Human can" and "Agent can" are kept from V2; plain-word alternatives ("You can") were rejected earlier. Confirm.
- **Q8.** Should the `--wv-*` handles (target floor, hairlines, durations, rail width) be promoted into house tokens, given the house rule that structural roles live in tokens and the target floor lives with Play-Nice?

**UNKNOWN**
- **Q4.** The real sandbox rules and browser support for embedded apps.
- **Production Connect API** and credential-reference grammar. The contract's references look like `env:`, `vault:` or `file:`; the prototype uses `ref://name` from V2. These must be aligned before a real build.
- **Whether Front Door is governed by Project Home** (so whether "Ask Project Home to review" is ever the right route for its proposal) is not in the fixture.
- **Project Home approval verification.** The contract notes the verify step has no implementation, so the path is fail-closed end to end. This design assumes "not available yet" until proven otherwise.
- **Q6.** What makes a service Stale in production (probably each read's time-to-live) versus the scripted event used here.
- **Live feed transport and cadence** in production.
- **Disconnect history policy:** whether anything is kept. The disabled "Keep history after disconnect" stays until decided.
- **Whether a fixture's facts map one-to-one to the contract's capabilities** (the scope list assumes they do for Notes Sync).
- **Q9.** Contrast, focus, reflow, screen-reader order and the checker are **not verified** by this document. Contrast numbers come from the tokens; they must be re-measured on the built prototype at desktop, tablet and phone sizes, in all four themes.
- **Dot in Light mode** (see §8.5) and the exact shell and body colours.

## Appendix: decisions in force (quoted)

- 2026-10-13 Rylee: "Yeah. V2 is the only one that passed. Can we now feed that back in with all the field guide info and everything to generate a full design?"
- 2026-10-13 Rylee: "comprehensive but also keep the original feeling." Clarified: protect the feel, the look, the whimsy and the love, NOT the copy; copy may be rewritten for clarity (LANGUAGE.md).
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee, Project Home decision 145: "Approve the boundaries; Sol's six conditions become pre-prototype gates" (the Connect contract, draft PR #329).
- 2026-10-12 Rylee: "Field Guide as base" (the World Tree Field Guide is the base of the shared design system; Mulberry & Mint).

None of these is contradicted by this document. Where it departs from V2's wording (§2), it does so inside the "copy may be rewritten" clarification or to honour decision 145.

