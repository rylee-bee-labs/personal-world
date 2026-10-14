# Worlds vNext real round: critique concept V3's DESIGN.md (another author's)

## Decisions in force
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee: "hmmmm, the direction of C... let's kick of the real rounds and have them keep in mind the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"
- 2026-10-13 Rylee: "take into account prompt guidance and the opendesign options to make unique things of course"

Read `docs/vnext/concept5-variations-brief.md`, `docs/vnext/concept5/DESIGN.md`, `docs/vnext/house/` and `docs/vnext/fixture/fixture.json`. Write `docs/vnext/c5var/V3/CRITIQUE.md` with AT MOST 5 gaps, most important first: what is missing or wrong (cite the brief), why it matters, the concrete fix. Focus on: real daily-use application (nav, settings, first-run, error state), whimsy and warmth laced in (not a theme park), two purposeful layouts (1440 and 390), reactivity (live feed, propagation, optimistic feedback, transitions, pause), integration requirements 1-6, honesty, faithfulness to Concept 5, a clearly distinct overall flow, correct use of the house design language (tokens, Mulberry & Mint, LANGUAGE.md words). Do not grade generously. Write only that file. Do not push.

---
## DESIGN.md for V3

# V3 — Ask the Counter (MOCKUP)

Status: MOCKUP. Synthetic fixture data only (`docs/vnext/fixture/fixture.json`); nothing here is a real reading. A visible "MOCKUP" label sits in the top bar (desktop) and the page header (mobile) on every screen, Connect and Settings included. Variation of Concept 5 (`docs/vnext/concept5/DESIGN.md`); nothing here is approved.

## 1. Metaphor in one line, and the main gesture

Worlds is a friendly service counter, and the counter **answers when you ask**. Every service hands its facts across it; you never have to go looking — you say what you want ("what needs me?", "open front door", "connect notes") and the counter puts the answer on the counter-top. Main gesture: **ask, then read the fact's journey** (Source → Worlds → Agent / You). Dot the snail sits at the right edge of the ask field: asleep when there is nothing to say, awake and listening while you type, and it nods once when an answer lands.

## 2. The flow

### Desktop, 1440px

1. **Open.** The ask field is at the top of the centre column, empty and focused-looking, with four suggestions under it: *what needs me?*, *connect notes*, *open front door*, *why is media shelf stale*. The answer pane below already holds the Front Door surface (selected by default, as in Concept 5) and its header line reads "Nothing asked yet — showing Front Door". Right column (360px, `--surface-raised`) is **At the counter**: the selected-service card (state word, fixture reason, *Open Front Door*, and **`#disconnect` "Disconnect Front Door" visible on the first screen**), then the other slips, then `#add-service`. Top bar: MOCKUP label, `#live-toggle`, `#simulate-change`, `#attention-count` "1 needs you".
2. **Ask.** Type. The suggestion list filters live to what the counter actually holds; typing a service name offers that service's surface. `/` (and ⌘K / Ctrl+K) focuses the field from anywhere.
3. **Answer.** The answer pane renders the resolved surface and stamps it: **"asked 12s ago"**, ticking with the feed. An answer ages visibly — that is the honesty rule of this variation (§6). `frontdoor.back-door` opens its receipt: three stations (Source → Worlds → Agent / You), then the *Agent sees* strip, then the app frame with its fallback link, exactly as Concept 5. `connect notes` runs Connect as a pane replacing the answer, steps 1–6 unchanged, `#step-back` / `#step-next` / `#run-test` / `#publish` real buttons. `what needs me?` groups the Needs-you set and the stale set under the words *Needs you* and *Stale*.
4. **A change arrives.** `#simulate-change` applies `lock-back-door`: the answer pane, the slip, the receipt stations, the *Agent sees* strip, Memory and `#attention-count` all re-render in one pass; Dot nods; the stations tick in order; "Mock handoff recorded; Worlds locked nothing." stays under the answer.
5. **Leave cleanly.** `#disconnect` opens the same confirm dialog (button labelled **Disconnect**, plus Cancel) and afterwards the slip is gone and the line "{Name}'s {n} facts and its app frame are removed from Worlds; the app itself is untouched." stays in the answer pane. The four destinations (Ask/Counter, Connect, Memory, Settings) remain reachable at every step; the ask is an accelerator over them, never a gate.

### Mobile, 390px (own control map, not the desktop order)

The ask field is the **first thing on the page**, not a bottom strip — that is what keeps the one-strip rule true at 390px.

| Screen | Bottom strip |
|---|---|
| Counter (ask + At the counter) | Tab bar, 56px + safe-area |
| Pushed answer / service screen | None; Back is in the top bar |
| Connect stepper | Sticky Back/Next replaces the tab bar |
| Disconnect dialog | Bottom sheet over the tab bar, scrim behind |

Order: page header (MOCKUP + title) → ask field + suggestions → status row (`#live-toggle`, `#simulate-change`, 44px, wrapping) → selected-service card with `#disconnect` on the first screen → remaining slips → floating `#add-service` pill, `bottom: calc(56px + env(safe-area-inset-bottom) + 16px)`, `right: 16px`, with 136px of bottom padding so it never covers a slip. Submitting an ask pushes a full screen: Back, the answer, "asked 12s ago", the receipt stations stacked in a column, the agent strip, the app frame with its fallback link. Suggestions are a plain vertical list of full-width 44px buttons, never a horizontal chip scroller; no sideways scroll anywhere; rows wrap, never truncate. Typing filters suggestions in place — no sheet competes with the keyboard or the tab bar.

**Keyboard-first, still tappable.** ⌘K / `/` focus; ↑/↓ move focus between the suggestion *buttons* (real focus, not `aria-activedescendant`); Enter activates the focused one; Escape clears the field and returns focus to whatever invoked it. Every suggestion is also reachable by Tab as an ordinary button, so the ask bar is a shortcut and never a keyboard trap. Focus moves to the answer heading on every answer, and Back returns focus to the suggestion that produced it. Escape closes dialogs and returns focus to the invoking button.

## 3. What changed from Concept 5, and why

| | Concept 5 | V3 | Why |
|---|---|---|---|
| Entry point | the composed counter; you pick a slip | one input; the counter is the **result** | Three services, four destinations and a tick every 5s make "the obvious next thing" change constantly. In Concept 5 a person hunts and re-hunts; asking "what needs me?" is the daily-use case Rylee named ("something she would use every day"), and it costs one keystroke. |
| Selection | pick slip → pick fact | ask → the answer *is* the service surface, with the fact journey opened in place | The receipt stays the hero and the signature, but it now arrives as an answer rather than being hunted for in a pane. |
| Freshness | every row shows its own age | the answer *also* shows when it was asked | A result surface that silently goes stale is the one dishonesty this flow could introduce. "asked 12s ago" makes a paused or old answer visible, and `prefers-reduced-motion` does not freeze it. |
| What is asked | n/a | never a decision | A typed "lock back door" answers "Worlds doesn't decide that. Open Home app." with the `app://home` link. Asking must not become a way around requirement 5. |
| Unchanged | slips, Dot, the receipt, six-step Connect, four destinations, honest rows, embedded frame, all states | | The variation is the *flow*, not a new concept. |

**Kept from C:** the persistent shell, owned and labelled services, honest states, the verbatim fixture reasons and fallbacks. **Differs from C in one testable interaction:** in C, selecting a bay docks an app beside a static fact card and nothing further happens per fact; here, a question resolves to a fact's three-station journey and the stations visibly re-settle when a change arrives, with the ask line, the slip, the receipt and the agent strip updating in one render. It is not a ledger: a ledger is a read-only typographic list; here every answer row is a disclosure with stations, a settle sequence and a per-fact permissions control.

## 4. House tokens (Mulberry & Mint, dark by default)

Copy the `:root` and dark blocks from `docs/vnext/house/tokens.css` inline; no values are invented. Page `--surface-base` #291d30, panels `--surface-raised` #352640, wells `--surface-sunken` #1f1625; text `--text-primary` #f5ecf2 / `--text-soft` #dacad9; lines `--border-line` / `--border-line-strong`. The one act on screen is `--action-primary` #a7e6ce with `--action-on-primary` #291d30 — used on the run button and the focus of the ask field only, never as a large surface, never beside mint. Sizes, gaps and radii come from the structural ladder (`--space-xs…--space-3xl`, `--radius-sm…--radius-full`, `--type-micro…--type-h1`), every length in `rem`. **Focus:** the product-local alias Worlds already ships (`--focus: #72b1b1`, the accessibility contract's teal station ring, `worlds-mulberry-mint.css`) is this variation's alias of `--focus-ring`, drawn at 2px with a 2px offset on the ask field, every suggestion, and every dialog control. **Type:** no external fonts — body `Atkinson Hyperlegible`, then system fallbacks; display `Young Serif`, Georgia, serif, for the answer heading and the four destination labels only. Dark is the default; `prefers-color-scheme: light`, `prefers-contrast: more` and `data-theme="low-stimulation"` come from the mode blocks as written; an explicit `data-theme` always wins. Soft contrast, never pure white on black; status is never colour-only.

**Words:** states are Play-Nice's, and the human labels are the house mappings only — Needs you (`needs_attention`), Working (`working`), Held (`deferred`), Done (`complete`), Failed (`failed`), Unknown (`unknown`); `stale` keeps the fixture's own word "Stale". No state is invented, here or in the answer surface. The reserved accessibility floor (targets, contrast numbers, motion limits) is Play-Nice's, read from the contract and never restated.

## 5. States (all of Concept 5's, plus the ask's own)

First run (zero services): Dot asleep, "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you.", and the ask offers exactly one suggestion — *connect notes*. Quiet: "That's the last one. Nothing needs you right now." Needs-you, Stale ("Nothing has changed since {time} that Worlds knows of." + *Check now*), Unknown ("Worlds has no current reading for this and won't guess."), Error ("That didn't go through, and the reference is still here. Try again whenever you like."; at the limit, "That's three tries. Check the reference with the owning app, then edit it to start again.") and Disconnected all behave as in Concept 5.

New to this flow, all honest and all in words:

- **Nothing typed:** the field's hint is "Ask the counter", and the answer pane says "Nothing asked yet — showing {service}" or, with nothing connected, the first-run line.
- **No match:** "Nothing at the counter answers to that. Try *what needs me?* or a service name." Never a fabricated result, never a fuzzy guess dressed as a fact.
- **Ambiguous:** "Three things here answer *sync*. Pick one." — the three are listed as buttons, not resolved for the person.
- **Loading:** static Dot and "Dot is fetching the facts…" with a recovery link to the fallback. **Empty preview:** "This service offers no facts yet. Nothing is guessed." (Local Weather). **Long copy:** wraps, never truncates.

## 6. The live feed (unchanged model, one addition)

Concept 5 §14 stands: mock clock anchored at fixture `generatedAt`, ages always recomputed from `checkedAt`, 5s tick (Settings: 10s / 30s), Front Door re-checks every 20s and Media Shelf every 30s, only a Healthy all-live service may age, one simulated Stale event for Media Shelf at 90s, `#live-toggle` (`aria-pressed`) pausing clock, ticks, re-checks and the staleness event while data and actions stay usable, every feed-driven change carrying the word "simulated", one store keyed by fact id and one render pass.

**Addition:** a tick re-filters the suggestion list and updates the ask's stale marker, and an answer's "asked Ns ago" keeps counting — but **the ask never auto-runs.** The counter does not answer a question nobody asked again; that is what keeps the flow trustworthy. When `#simulate-change` changes the attention count, the *what needs me?* suggestion gains its count in words ("what needs me? · 1") and the answer line reads "Updated just now" for 3s. Pausing the feed does not freeze the answer's age; it freezes the data, and the two are distinguished in words.

## 7. Warmth and motion

Dot's role is unchanged in kind (reacts, never carries the tick) and shaped for this flow: asleep at the field's right edge when the field is empty, a listening pose while typing, a single 200ms nod when an answer lands, and it carries the parcel on connect as in Concept 5. The warmth is the verbatim Concept 5 lines plus two new ones — "Nothing at the counter answers to that." and "asked 12s ago". One curve, `cubic-bezier(.2,0,0,1)`; 150ms confirmation (120ms mobile), stations staggered 60ms, sheets 200ms. Under reduced motion: no transform, no Dot travel or nod, changes swap instantly with opacity, stations show all ticked at once, and the numbers keep updating. A person can ignore every dot and nod and still finish the job.

## 8. Integration requirements (1–6, all in the build)

1. **Plug-in:** `#add-service` opens Connect for `notes-sync` — Discover, Credential reference (a pasted secret is refused with "Worlds stores a credential reference, never the secret"), Safe test (with the labelled "Simulate a failed test (mock)" checkbox), Preview, Choose, Publish. Dot delivers the parcel, a Connected stamp lands, Notes Sync appears in the slips, in Memory and in Settings. Askable as "connect notes" or by button — the same flow either way.
2. **Composition:** three services from the fixture, ids, names and states verbatim.
3. **Embedded app frame:** drawn bordered frame headed by owner and state word, a secondary strip under the receipt, never the hero, with the fixture's fallback verbatim ("Open Media app" / "Open Home app" / "Open Memory app"), and on failure the shell, facts and fallback stay with the cause "This app didn't load inside Worlds."
4. **One fact, many places:** `#simulate-change` updates answer pane, slip, receipt, agent strip, Memory and Settings at once.
5. **Governed action hands off:** approvals to Project Home, everything else to the owner, and a decision is never an askable action (§3).
6. **Disconnect:** `#disconnect` on the first screen, confirm dialog, honest line, Worlds still works.

**Checker ids:** `#add-service`, `#disconnect` (visible on the first screen for the selected service), `#simulate-change`, `#live-toggle`, plus the visible MOCKUP label.

## 9. OpenDesign starting point, signature, lock, risks

Skill `frontend-design`, design system `ds-rylee-bee-studio` with the house token sheet and `worlds-mulberry-mint.css`; craft rules applied: state-coverage (all five states per surface, including the ask's no-match and ambiguous cases), accessibility-baseline, animation-discipline, anti-ai-slop (no emoji icons, no gradients, no invented metrics), plus color and typography from the house. **One signature, unchanged from Concept 5: the fact journey receipt.** The ask field is the entry gesture, not a second signature; the counter is its result surface. **Reference lock (mechanics only):** Linear's command palette — prefix-free filtering, grouped results, full keyboard operation, no second modal — and Apple Home's accessory states. Deliberately *not* the round-2 sentence-first or key-ring looks: an answer here is never a line of prose, it is a real surface with stations and permissions. **Risks and UNKNOWNs:** embed support and isolation, the Connect API, whether disconnect retains history, system-font fidelity, and — specific to this variation — whether filtering feels like a search box (wrong) or like a counter that knows what it holds (right); the fix, if it feels wrong, is more of the four named suggestions and fewer free-text affordances, never fuzzy matching.
