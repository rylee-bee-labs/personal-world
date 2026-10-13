# THE DESK — a Worlds vNext concept (round 1, concept 2 of 3)

**MOCKUP.** A concept prototype for comparison. Not built, not approved, not
wired to anything. Open [`index.html`](./index.html) in a browser — no server,
no build, no network, no fonts, no images.

## The one-line idea

Worlds is one sheet of writing that is always being rewritten: you do not
navigate it and there is no menu, you **put the caret on a line** and the line,
its margin note and the tally all answer in one pass.

## The main gesture

**The caret.** There is no sidebar, no page, no route, no card grid. The whole
front door is a sheet of text with a gutter. `Tab` to it, then `↑`/`↓` walk the
caret from service line to service line; `Enter` opens the line under the
caret, which *grows a paragraph* inline underneath itself. Clicking or tapping
a line puts the caret there too. Typing in the box above the sheet filters it
live — non-matching lines collapse to a dotted `·` gutter mark and the sheet
tells you how many it hid.

Information architecture: one sheet, three layers that can never disagree —
the **lines** (the fixed sample facts), the **margin** (source, freshness, and
the two permissions, set as narrow notes to the right of each line), and the
**journal** (an append-only record of what you just did, written into the same
sheet). Everything is recomputed from one state object by one `render()`, so a
change that lands in one layer cannot forget another.

## The three interactions

1. **Put the caret on a line** — arrow keys, click or tap. Live and shared:
   the caret bar, the status line under the sheet (which reads the line's state
   *as a word*), and the highlighted margin note all move together.
2. **Answer the backups fact in place** — *this is the interaction that updates
   several views at once.* The line under the caret opens; the Backups line is
   the only one waiting on a person, so it offers three real answers (retry the
   copy tonight / keep both copies / leave it and note it on the board). One
   click rewrites **five** surfaces in a single commit: the Backups line's own
   text, the `you may` line in its margin, the `##` tally at the top of the
   sheet, the `## journal` block that gains a timestamped note, and the live
   region. The agent's line changes too — it stays `read, report`, because the
   agent never gained the right to decide anything.
3. **The embedded Tape Deck** — the media player is drawn *inside* the sheet, in
   the sheet's own typeface, with a playhead, a waveform and transport.
   Play/Pause steps the playhead; **Finish this episode** resolves a shared
   fact, and the media-library line, the top tally and the journal all change
   together. A "Rewind" button sits beside it for people who would rather not
   wait 41 minutes.

Two smaller ones ride along: typing filters the whole sheet, and `Esc` closes
an open line and returns the caret to the sheet.

## What is embedded, and the way out

| Embedded app | Owner | Fallback link |
| --- | --- | --- |
| **Tape Deck** — queue, waveform, playhead, transport | Media library | `https://example.invalid/apps/media-library/episodes` |

The player names itself as an embedded app, states in plain words that it is
drawn inside Worlds and talks to nothing, and keeps the "open in the owning
app" link in its own title bar — visible at all times, never behind a
gesture. Embedding is the convenience; the link is the supported path when a
provider cannot or will not be embedded.

## Honesty rules the prototype keeps

- **Files** is *stale*: `[stale] · last heard 3 hours ago — Worlds will not fill
  the gap`. It is never upgraded to fine, never to unknown, and never to error,
  and there is no "refresh" button that would fake a fresh reading.
- **Weather** is *unknown*: `[unknown] · never answered · · · · no reading is
  drawn`. Its margin reads `observed_at: none — this provider has never
  answered`, and **both** permission lines read *nothing — there is no fact to
  act on*, because there is nothing to permit.
- Every state is a **word first**: `fine`, `stale`, `unknown`,
  `needs a human decision`. Colour is only ever additional, and each state also
  differs in type treatment — plain, dashed underline, italic, reversed — so
  the sheet still reads correctly in monochrome or to a screen reader.
- Every reading is the brief's fixed sample, dated in the sheet head
  (`# MOCKUP — worlds desk · sample snapshot, Tue 13 Oct 08:14`). Ages are
  computed from `observed_at` at render time against a clock that starts at the
  snapshot, so a stale fact gets *older* while you watch and nothing is ever
  silently refreshed. The sheet says how old the session is.
- `agent may` and `you may` are separate runs of text on every line and every
  open paragraph. The agent reads all of it and acts on none of it without you.

## Phone, keyboard, motion

- **390px:** one column. The gutter narrows, the margin note drops underneath
  its line with a dotted left rule, the waveform wraps rather than scrolling,
  and the sheet never scrolls sideways — every string wraps.
- Every control is a real `<button>` or `<a>` at 44px minimum height, with a
  2–3px visible `:focus-visible` ring. The sheet itself is a focusable group
  with instructions in `aria-describedby`.
- Sheet keys only act while the sheet holds focus, so a decision button keeps
  its own `Space` and `Enter`. `Esc` closes an open line from anywhere inside
  the sheet. Focus survives a full re-render: playback repaints the waveform
  instead of rebuilding, and after a decision focus returns to the sheet.
- `prefers-reduced-motion: reduce` disables all transitions; the playhead
  still advances, in visible steps rather than a smooth tick.
- A `role="status"` live region announces the caret line, every answer, and
  every resolved episode in words — including both permission lines.

## What a Connect API would have to supply

- **One observation shape per fact**, not per dashboard: `service`,
  `fact_key`, `source`, `observed_at` (UTC), a value or a sentence, and a
  **state** that is explicitly one of `fine | stale | unknown`. `unknown` must
  be a first-class value a provider can return; a missing body must never be
  rendered as an empty reading.
- **`stale_after` from the provider**, so "stale" is the provider's own word
  and not a guess made by the front door.
- **Freshness once, in one place.** `observed_at` is the only clock; Worlds
  derives the age at render time and keeps no second opinion.
- **Two permissions, returned separately**: `agent_may`
  (read / report / draft) and `owner_may` (act / decide). They must not be
  flattened into one "accessible" flag — the whole sheet is built on their
  being different.
- **An app manifest for embedding**: `app_id`, `owner`, `embed_url` (or an
  explicit "cannot embed"), `open_url` for the fallback, and the frame
  capabilities it needs. The fallback link is drawn from `open_url` alone, so a
  provider that refuses embedding still ships a first-class surface.
- **Actions as governed requests, not buttons**: an answer posts back as
  request → approve → dispatch-at-most-once with a returned
  `SUCCEEDED | FAILED | UNKNOWN`, so a decision that quietly fails returns as
  *unknown* rather than as a line that lies about being resolved.
- **A subscription feed** (SSE or Web Push) of changed facts — events naming
  which fact changed, so a live sheet can rewrite one line instead of the page.

## Files

- `index.html` — the whole prototype: inline CSS and JS, inline SVG-free
  (CSS-drawn waveform), no dependencies, no network.
- `README.md` — this page.