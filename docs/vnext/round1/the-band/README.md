# THE BAND — a Worlds vNext concept (round 1, concept 3 of 3)

**MOCKUP.** A concept prototype for comparison. Not built, not approved, not
connected to anything. Open [`index.html`](./index.html) in a browser — no
server, no build, no network, no fonts, no images. Inline SVG only.

## The one-line idea

Your services are stations on one tuning dial and Worlds is the receiver: you
turn the dial, the front door retunes around whichever service is talking, and
what you hear is the same fact your agent hears.

## The main gesture

**Tune.** There is no sidebar, no list, no page and no menu. One rotary dial
carries six detented stations; you drag the needle anywhere on the dial face,
click a station, click its line in the band strip, or focus the dial and press
<kbd>←</kbd>/<kbd>→</kbd>. The needle follows your hand continuously, and the
moment you are within a detent it **locks** — the station's tick fills, the
dial core names it, and the tuning window retunes. Park the needle between two
detents and the receiver says *no carrier* rather than quietly showing you the
nearest station.

Information architecture: one dial, and three surfaces that always agree —
the **ticks** around the dial (a fact's state, drawn as a border and a glyph),
the **tuning window** (the locked station in full: source, freshness,
permissions, and its embedded app if it has one), and the **band strip** (all
six stations at a glance, each in words, which is also how you tune). A
**transmission log** below records everything you did. All four are recomputed
from one state object by one `render()`.

## The three interactions

1. **Tune** — drag the needle, click a tick, or use the arrow keys. Live and
   shared: the needle, the dial core's words, the tuning window's contents, the
   highlighted line in the band strip, and the live region all move together.
   Stop between stations and the window replaces itself with *no carrier*.
2. **Answer the Backups transmission** — *this is the interaction that updates
   several views at once.* Tune to Backups and three real answers appear
   (retry the copy tonight / keep both copies / leave it and note it on the
   board). One click rewrites **six** surfaces in a single commit: the tick's
   border and glyph on the dial, the tuning window, the `you may` line (which
   becomes *answered by you at 08:14*), the header tally (*1 waiting on you*
   becomes *0*), the band strip entry, and the transmission log. The agent's
   line does **not** change: it still reads `read, report`, because reading was
   never the thing that was missing.
3. **The embedded apps, inside the tuning window** — tune to Media library for
   the **Tape Deck** (playhead, VU meter, transport; *Finish this episode*
   resolves a shared fact and moves the dial tick, the band strip, the tally
   and the log together), or tune to Home lights for the **Switchboard** (7
   bulbs). Flipping a bulb does **not** claim the light changed: the bulb flips
   to a dashed "asked — no answer yet" state and stays there, because the
   provider has not reported back.

A fourth, smaller one: the **What I can see / What the agent sees** switch in
the tuning-window header swaps the same fact between the human rendering and
the shape the agent receives (`service`, `fact_key`, `state`, `observed_at`,
`source`, `agent_may`, `owner_may`, `human_readable`).

## What is embedded, and the way out

| Embedded app | Owner | Fallback link |
| --- | --- | --- |
| **Tape Deck** — queue, VU meter, playhead, transport | Media library | `https://example.invalid/apps/media-library/episodes` |
| **Switchboard** — 7 bulbs | Home lights | `https://example.invalid/apps/home-lights` |

Each app names itself as an embedded app, states that it is drawn inside Worlds
and talks to nothing, and keeps its **open in the owning app** link in its own
bar, always visible — never behind a gesture, never only on failure. Embedding
is the convenience; the fallback link is the supported path when a provider
cannot or will not be embedded.

## Honesty rules the prototype keeps

- **Files** is *stale*: its tick has a **dashed** border and its window says
  "last heard 3h 3m ago", with an explicit note that stale is neither unknown
  nor an error. There is deliberately **no refresh button** — a button that
  pretended to re-hear the provider would be a lie. The age keeps growing
  while you watch.
- **Weather** is *unknown*: a **dotted** tick, "never answered", and both
  permission lines read *nothing — there is no fact to act on*. Nothing is
  drawn in place of a temperature.
- Between stations the receiver says **no carrier** and shows nothing, rather
  than falling back to the nearest station and implying it is tuned in.
- Every state is a **word first** — `answering fine`, `needs a human`, `stale`,
  `unknown` — in the tick's accessible name, the band strip, the dial core, the
  state pill and the log. Colour is only ever added on top, and the four states
  also differ in border style (solid / double / dashed / dotted) and glyph
  (■ / ▣ / ▨ / ◇).
- Every reading is the brief's fixed sample, dated in the header. Ages are
  computed from `observed_at` at render time; the header shows the receiver
  clock and how long the session has been open.
- `agent may` and `you may` are separate rows on every fact, in both views.
  The agent reads all of it and acts on none of it without you.

## Phone, keyboard, motion

- **390px:** one column — dial, tuning window, band strip, log. The dial
  scales to `min(20rem, 92vw)` and its six station buttons stay 52px circles,
  well over the 44px minimum, with real pressed and focus states. Tables,
  meters and app bars wrap rather than scroll; the page never scrolls sideways.
- The dial is a `role="slider"` with `aria-valuemin/max/now` and an
  `aria-valuetext` that reads the station, its state *and* its age in words. The
  six station buttons are separate real buttons with 44px+ targets and visible
  `:focus-visible` rings; band-strip entries and bulbs are buttons too.
- Focus survives every rebuild: whatever you had focused keeps its place when
  the window is recomposed, and answering a fact returns focus to the dial so
  you can keep tuning.
- `prefers-reduced-motion: reduce` removes the needle's transition — it jumps
  straight to the detent instead of sweeping. The playhead still advances, in
  visible steps.
- A `role="status"` live region announces every lock, every release, every
  answer and every asked-for bulb change in words.

## What a Connect API would have to supply

- **One observation shape per fact**, not per dashboard: `service`,
  `fact_key`, `source`, `observed_at` (UTC), a value or a sentence, and a
  **state** that is explicitly one of `fine | stale | unknown`. `unknown` must
  be a first-class value a provider can return; a missing body must never be
  rendered as an empty reading.
- **`stale_after`,** so "stale" is the provider's own word and not a guess made
  by the front door, plus the fact that a provider that has never answered is
  *not* the same as one that answered once and went quiet.
- **Freshness once, in one place.** `observed_at` is the only clock. The dial,
  the band strip and the log all derive the age at render time from it and keep
  no second opinion.
- **Two permissions, returned separately**: `agent_may` (read / report /
  draft) and `owner_may` (act / decide). They must not be flattened into one
  "accessible" flag — the whole receiver is built on their being different, and
  the agent view is only meaningful because it can show them apart.
- **An app manifest for embedding**: `app_id`, `owner`, `embed_url` (or an
  explicit "cannot embed"), `open_url` for the fallback, and the frame
  capabilities it needs. The fallback link is drawn from `open_url` alone, so a
  provider that refuses embedding still ships a first-class surface.
- **Actions as governed requests, not switches**: a bulb press posts back as
  request → approve → dispatch-at-most-once with a returned
  `SUCCEEDED | FAILED | UNKNOWN`, and the surface must be able to sit in the
  "asked, not yet answered" state the Switchboard holds. A switch that quietly
  fails returns as *unknown*, not as a bulb that lies about being on.
- **A subscription feed** (SSE or Web Push) of changed facts, so the band strip
  and the dial can update the station that changed without a page refresh.

## Files

- `index.html` — the whole prototype: inline CSS and JS, one inline SVG dial,
  no dependencies, no network.
- `README.md` — this page.