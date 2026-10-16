# Concept B — "The Garden map"

**Status: MOCKUP.** One of several concepts for Rylee to compare side by side. Nothing here is
approved, deployed or wired to a service. All data is synthetic. No app code, and no v3.1
prototype file, was changed to make this.

## The idea in three lines

Home is a small illustrated garden, and every connected service is a spot in it.
Tapping a spot opens its card: on a phone a bottom sheet over a scrollable map, on desktop a
map on the left and a detail pane on the right.
The other three screens (Connect, Memory, Settings) keep the same house, so the whole thing can
be walked end to end and compared against v3.1.

## What is distinct from v3.1

| | v3.1 (the counter) | B (the garden) |
|---|---|---|
| Home's shape | A vertical list of slips on a rail, with the receipt in a pane | A spatial map. Position is the navigation; the list exists only as the spots on it |
| Where a service lives | A row in a list you scroll | A place you can point at. Two of a kind ("Shelf Stack 2") reads as two places, which the counter list made hard |
| Selecting | Clicking a slip | Walking to a spot. The detail pane / sheet is the same card either way |
| Empty | A dashed well in a list of slots | A drawn garden with nothing in it, and one dashed plot waiting to be filled |
| Empty plot for an unconnected service | Not shown until you go looking in Discover | Drawn on the map from the start ("Rain Gauge · Not connected · nothing read") |

Kept on purpose, because they are Rylee's standing rules, not this concept's invention:
Dot the snail and its warmth (`dot.webp`, `dot-asleep`, `dot-curious`, `dot-settled`, `stamp.webp`,
`empty-counter.webp`, `empty-ledger.webp`, `dot-peek.webp`, all by relative path into
`docs/vnext/prototype/art/`), the terse-warmth voice, and the house Mulberry & Mint tokens copied
verbatim from the v3.1 prototype so the two can be diffed by eye.

## The Connect screen follows the real flow

Discover → Reference → Test → Preview → Choose → Publish → receipt, in the contract's order
(`docs/vnext/connect-contract.md` §1). It shows what each candidate says it can read and marks
every capability *unverified* until a test has run; the reference scope is a visibly different
list from the publish selection; the receipt has the three stations (Source → Worlds → Agent /
Human). Honesty the brief asked for is on screen rather than implied:

- **Unscoped** is labelled unscoped, in Connect (older connection) and in Settings, with a
  "Narrow scope" way out.
- **Unknown** says Unknown. No reading is invented and no blank is dressed up as a value.
- **Auth: unknown** is said when a probe does not answer, instead of assuming "none".
- A pasted secret-shaped string is refused, cleared and not echoed.
- Approvals are not connected and nothing claims one was sent; a proposal has no Run button.

## Known gaps

1. **No live settling.** A fact going stale or unknown while you watch is not animated here, so
   the "walk out and come back" feeling of the concept is unproven.
2. **No multi-instance proof.** The map can show two spots for one kind, but nothing creates one.
3. **Connect does not write to the garden.** Publishing shows the receipt and links to Home, but
   the garden does not actually grow a new spot — the wiring is out of scope for a mockup.
4. **The map does not scroll or pan.** On a phone it is one screen tall; a garden with 20 spots
   would need a scrollable or zoomable plan, and that is undecided.
5. **No app frame, no embedded view.** v3.1 has one; B leaves it out, which is a real difference
   to judge rather than an oversight.
6. **State switching is per screen and global to the page.** There is no single "one world, four
   screens, one state" model, because that is what v3.1 already shows.
7. **Not checked with a screen reader.** Structure, focus order, live regions and targets were
   written to the house rules; nobody has driven it with NVDA, VoiceOver or a switch device.
8. **Synthetic times are hardcoded** ("checked 24s ago"). There is no mock clock, so the age line
   never grows.

## Files

- `index.html` — the whole concept in one self-contained file: inline CSS and vanilla JS, no
  network, no CDN, no external fonts. Art is referenced by relative path only.
- `README.md` — this file.

## Checks run

See the commit message. In short: no `innerHTML` anywhere in the file, all four screens reachable
by keyboard, all four state switchers present, every referenced art path resolves, and the token
pairs used for text clear 4.5:1 in both the dark and light themes.