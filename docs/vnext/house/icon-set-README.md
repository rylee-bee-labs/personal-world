# Icon set: B Classic sticker

**Status: owner-picked style, 2026-10-12.** Rylee chose style B ("Classic
sticker") from the six icon packs in `media_files/icons/` as the one icon
style for every app. These 42 icons are cut from `iconpack2.png` and are
256 px transparent PNGs. `manifest.json` lists each icon's id, file and the
box it was cut from, so any icon can be re-cut from the source.

## What is here

| Group | Icons | Use for |
|---|---|---|
| `crew-*` | Scoop, Hekek, Mira, Ratatoskr, Renai, Solace, Bolt, Bruma, two poses each | Companion avatars and the "who is talking" chip. Presence, never machine status. |
| `place-*` | food truck, observatory, library, bridge, quarters, workshop | Place cards and wayfinding |
| `object-*` | books, star book, star map, lantern, shell, wave, jellyfish, coral, treasure, compass | Empty states, collections, achievements |
| `ui-*` | planet, heart, star, gear, folder, people, bell, search, anchor, wave | Controls (suggested meaning below) |

Suggested `ui-*` meanings: planet = Explore, heart = Favorite, star = Guide or
highlight, gear = Settings, folder = Files, people = Crew or people, bell =
Notifications, search = Search, anchor = Pinned, wave = Activity. A painted icon
always sits beside its word; it never carries meaning alone.

## The whole vocabulary

`vocabulary.json` maps meanings to pictures and carries the rules. Pack B has
no Home, Add, Close, Back, Edit, Delete, Calendar or Chat, so those reuse the
2026-10-04 painted icons in `assets/painted-icons/`, which share B's sticker
border and gold/slate palette. No new art was generated.

- **Ids are stable.** `action-*`, `status-*`, `crew-*`, `place-*`, `object-*`.
  Change a picture by changing its `src`, never the id.
- **Always beside a word.** The `<img>` is `alt=""` and `aria-hidden`; the
  label or the control's accessible name carries the meaning.
- **Smallest size.** Actions 20 px, crew 24 px, objects 32 px, places 48 px.
  Below that, use the product's own simple line glyph (Worlds' icon sprite,
  Project Home's lucide). Painted art does not read at 16 px.
- **Status is a word.** The Play-Nice word plus its status colour token.
  Painted status pictures exist only for Held and Failed and are optional.
  Needs you, Working, Done and Unknown have no picture on purpose.
- **Crew is presence, not state.** A crew pose says who is here or talking.
  A sleepy pose never stands in for Held.
- **Motion.** Icons are still. Any hover lift is off under
  `prefers-reduced-motion`.

## Delivery copies

`delivery/<id>-<64|128>.webp` are small copies for apps (1x and 2x for 32 and
64 px display), about 3–10 KB each against about 85 KB for a 256 px master.
Build them with `uv run --with pillow python assets/icon-set/build_delivery.py`.
The masters are never rewritten by it, and Pillow is only needed to run it,
not by anything that consumes the set.

## Visual check (2026-10-12)

All 42 icons and the 24 vocabulary pictures were rendered at 16, 20, 24, 28
and 112 px on Mulberry & Mint Dark base, raised and sunken, Light base and
High contrast, and looked at.

- **Recut.** The first cut kept specks from neighbouring icons. Each icon now
  keeps only the parts touching its own solid core, with a uniform 5% margin.
  No icon touches its frame edge.
- **Solace and Bruma.** Both pairs overlap in the source, so a straight cut
  left neighbour fragments and a hard edge. Fragments are removed and the cut
  edge fades over a few pixels. The ring tip of `crew-solace-hello` and the
  left rim of `crew-solace-love` are softer than the rest; a redraw is the
  only full fix.
- **Small sizes.** Crew faces stop reading under 24 px. `ui-people` (navy) is
  weak on dark plum under 24 px. `action-menu` does not read as a menu at any
  size and `action-chat` is soft under 24 px; their notes say so.

## Provenance

- **Source:** `media_files/icons/iconpack2.png` (estate root), in the media
  library since 2026-09-18. Its maker is not recorded there; the characters
  are Rylee's own crew.
- **Cut:** connected parts of the solid alpha, with fused pairs split at the
  emptiest column, then trimmed, squared on transparent and scaled to 256 px.
  No repainting.
- **Checked:** contact sheet on the Mulberry & Mint `surface/base` at 112 px
  and 28 px, 2026-10-12.
