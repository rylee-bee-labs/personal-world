# Art sources

**Status: inventory, 2026-10-12.** Where the system's artwork lives and how it
plugs into the Mulberry & Mint base. Counts come from the estate-root
`media_files/` tree on that date; the tree wins if they drift.

## The rule

`media_files/` (in the private estate-root repo) is the one media library.
Its bytes are kept by `media-archive` (content-addressed, never in Git). This
repo **references** that art by its `media_files/` path and copies a file in
only when a token, specimen or package needs it at a fixed size, with a
`PROVENANCE.md` beside it. A copy here is never a second place to edit the
original.

## What exists

| Kind | `media_files/` path | What is there | Use in the system |
|---|---|---|---|
| Painted icon packs | `icons/iconpack1.png` … `iconpack6.png` | Six 1536×1024 sprite sheets: crew portraits, rooms, objects, UI glyphs (home, folder, search, bell), sea and gem sets. Navy/teal/gold painted style. | Source for the painted icon set. Needs slicing into single transparent PNGs before use. |
| Character icons | `icons/*icon1.png`, `icons/renai-icon-badge.png` | One round icon per crew member (Renai, Bolt, Ratatoskr, Scoop, Solace, Hekek, Bruma, Mira) | Companion avatars and the "who is talking" chip |
| Painted UI icons (here) | this repo `assets/painted-icons/` | Status set (Held, Failed), everyday set (6), nav/chat/file set (27) | Already the system's icons; see its `PROVENANCE.md` |
| App icon | `icons/app-icons/` | Project Worlds planet emblem, PNG 16–1024 px plus `.ico`, navy `#051437` ground | Favicon/app-icon pattern for every product |
| Vectors | `svg-vectors/` | Character vectors (Bolt, Renai, Ratatoskr, Scoop, Solace), planet emblem, and theme sheets (`rylee-lab-icon-system.svg`, `rylee-theme-pack-framework.svg`) | Editable sources; the theme sheets are reference, not tokens |
| Characters | `characters/<name>/` | 164 images: 8 crew, crew scenes, VEFR residents, six-pose emotion sheets (`REST · CURIOUS · ATTENTIVE · ENGAGED · PROTECTIVE · GIVING SPACE`) | Companion and empty-state illustration; the pose sheets map to presence, never to machine status |
| Backgrounds | `backgrounds/`, `backgrounds/rooms/` | Brand heroes and room renders (bridge, engineering, observatory, map room, quarters) | Hero and place art for Worlds-family products |
| Fonts | `fonts/` | woff2: IBM Plex Sans 400/500/600, Plex Mono 400/500, Spectral 400–600, Fraunces, Cormorant 600, Space Grotesk 600 | Candidate type families. The Field Guide uses Inter; Playfair Display (the earlier heading pick) is not in the library. |
| Design screens | `designs/` | 1,711 files: finished screens for portfolio, VEFR and Personal World, the theme studio (7 themes), an older Personal World `DESIGN.md` + Station `tokens.css` | Evidence and reference. The Station `tokens.css` is the retiring Worlds kit, not a second token source. |

## Fit with Mulberry & Mint

- The painted art was drawn for navy, teal and honey. On plum it still reads
  (the sticker border separates it), but slate-blue fills sit closer to the
  surface. Check each icon at 28 px on `surface/base` and `surface/raised` in
  all four modes before it ships.
- Mint marks "selected" and Done; apricot marks evidence and Needs you; honey
  stays an accent (`accent/honey`). Painted icons always sit beside a word.
- Character art is presence, not status: a sleepy Bolt never means Held.

## Next

1. Slice the six icon packs into single icons with names and provenance.
2. Pick the type pair for the base (Inter from the Field Guide, or a library
   font) and add font tokens.
3. Switch `generator/` from teal to Mulberry & Mint once the specimen is
   reviewed.
