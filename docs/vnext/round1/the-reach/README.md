# THE REACH — a Worlds vNext concept (round 1)

**MOCKUP.** A concept prototype for comparison, not built, not approved. Open
[`index.html`](./index.html) in a browser — no server, no build, no network.

## The one-line idea

Worlds is one surface with one seam down the middle: your services hang on a
single thread, everything answers to where your hand is, and **a fact only
changes your world when you carry it across the seam**.

## The main gesture

**Reach, then carry.** Your pointer (or your focus) is a hand moving along the
thread: nearby marks lean toward it, the thread follows them, and that
service's rows light up in the shared facts table. Then you take a single fact
and carry it across the seam from the agent's side to yours — by dragging it,
or by pressing <kbd>Enter</kbd> on it. Nothing else in the interface is a
navigation act; there is no sidebar, no page to change, no menu.

Information architecture: one page, three surfaces that can never disagree —
the **thread** (six marks, in words), the **seats** on your side, and the
**shared facts table** (the rows an agent reads, rendered in front of you).
Every surface is recomputed from one state object by one function, so a fact
that moves cannot update in one place and forget another.

## The three interactions

1. **The reach** — proximity, live. Moving the pointer along the thread makes
   the nearest marks lean toward your hand, redraws the SVG thread through
   their new positions, and highlights that service's rows in the shared facts
   table. Keyboard: <kbd>Tab</kbd> into the thread, <kbd>↑</kbd>/<kbd>↓</kbd> to
   walk the reach from mark to mark; the same highlight follows focus.
2. **Carry a fact across the seam** — *this is the interaction that updates
   several views at once.* Dragging a fact chip onto your side (or activating
   it with the keyboard) changes **four** things in one commit: the mark on the
   thread drops its chip and reads "carried over"; the fact's row in the shared
   facts table re-badges its side; the **agent may** / **you may** columns are
   re-read next to it; and the headline recomputes ("1 fact is on your side:
   Backups"). A fact that arrives on your side grows the decisions that belong
   to you — the three Backups choices are real buttons that resolve the fact in
   place, in the ledger and in the headline. Any fact can be carried back.
3. **The embedded Tape Deck** — play the two new episodes inside Worlds. The
   transport runs, the playhead and waveform move, and finishing an episode
   resolves a shared fact: the queue badge, the Media mark, the shared facts
   row and the headline all change together. (A "Finish this episode" button is
   there so the effect is watchable in ten seconds rather than a minute.)

Two smaller ones come along for the ride: the Switchboard (Home lights) writes a
brand-new fact onto your side every time you flip a bulb, and <kbd>Esc</kbd>
abandons a carry in flight.

## What is embedded, and the way out

| Embedded app | Owner | Fallback link |
| --- | --- | --- |
| **Tape Deck** — media player, queue, waveform, transport | Media library | `https://example.invalid/apps/media-library/episodes` |
| **Switchboard** — 7 bulbs, switchable | Home lights | `https://example.invalid/apps/home-lights` |

Each embedded panel names itself as an embedded app, states plainly that it is
drawn inside Worlds and talks to nothing, and always shows the "open in the
owning app" link beneath it. Embedding is a convenience; the fallback is the
supported path when a provider cannot or will not be embedded, and the link is
never hidden behind a gesture.

## Honesty rules the prototype keeps

- **Files** is *stale*: its row says "stale — last heard 3h 3m ago" and the word
  is repeated in the headline. Staleness never becomes unknown, and never
  becomes an error.
- **Weather** is *unknown*: "unknown — never answered". No temperature is
  invented anywhere, and both permission columns read "nothing", because there
  is no fact to act on.
- State is always a **word**, and only ever *also* a colour; the styles differ
  by border style too (solid / dashed / dotted), so colour is never the signal.
- Every reading is the fixed sample snapshot from the brief, dated in the header
  and in the table caption. The header counts the minutes that have passed since
  the snapshot was drawn; the ages grow with it.
- **Agent may** and **you may** are separate columns and separate lines on every
  seat. The agent can read all of it and act on none of it without you.

## Phone, keyboard, motion

- 390px: one column, the seam turns horizontal and the thread collapses to a
  straight line, the shared facts table becomes stacked labelled records, and
  the page itself never scrolls sideways.
- Every control is a real `<button>` or `<a>`, at least 44×44px, with a visible
  `:focus-visible` ring. The carry has a full keyboard equivalent; focus follows
  the fact across the seam and is restored across every recompose.
- `prefers-reduced-motion: reduce` disables the lean, the FLIP travel and the
  thread redraw; the transport still advances, in visible steps instead of a
  smooth tick.
- A live region announces every carry, decision and resolution in words,
  including both permission lines.

## What a Connect API would have to supply

Nothing here needs a new concept; it needs honest observation.

- **One observation shape per fact**, not per dashboard: `service`, `fact_key`,
  `source`, `observed_at` (UTC), `value_or_text`, and a **state** that is
  explicitly one of `fine | stale | unknown` — plus `stale_after`, so "stale" is
  the provider's own word rather than a guess made by the front door. `unknown`
  must be a first-class value the provider can return; a missing body must never
  be rendered as an empty reading.
- **Freshness, once.** `observed_at` is the only clock. Worlds derives the age at
  render time (as this mockup does) and never stores a second opinion about how
  old something is.
- **Two permission columns, computed server-side and returned separately**:
  `agent_may` (read / report / draft) and `owner_may` (act / decide), because
  the whole point is that these are not the same and must not be flattened into
  one "accessible" flag.
- **An app manifest** for embedding: `app_id`, `owner`, `embed_url` (or an
  explicit "cannot embed"), `open_url` for the fallback, and the frame
  capabilities it needs. Worlds draws the fallback link from `open_url` alone,
  so a provider that refuses embedding still ships a first-class surface.
- **Actions as governed requests, not buttons**: a carried fact's decisions
  post back as request → approve → dispatch-at-most-once with a returned
  `SUCCEEDED | FAILED | UNKNOWN`, so a switch that quietly fails returns as
  unknown rather than as a bulb that lies about being on.
- **A subscription feed** (SSE or Web Push) of changed facts. The live reach in
  this prototype is fed by the same recompose function; the real one needs
  events naming which fact changed, not a full page refresh.

## Files

- `index.html` — the whole prototype: inline CSS and JS, inline SVG, no
  dependencies, no network.
- `README.md` — this page.