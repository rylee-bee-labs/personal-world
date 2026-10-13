# Rylee-Bee Design System

> Canonical shared presentation for the Rylee-Bee estate: token values, the naming rules, and pattern contracts. No component library.

## What this is

Three things:

- **Token values**, canonically, in [`tokens.css`](tokens.css).
- **The rules a shared surface follows** — naming, the modes, what a pattern
  must contain.
- **A pointer to Play-Nice** for the accessibility floor, which is Play-Nice's
  to own.

## What this is not

- **Not a component library.** No product imports component CSS from here. Each
  product implements its own (design-system#25): Studio keeps its `wk-` classes,
  Project Home keeps its React components.
- **Not a theme pack.** Product personality is product-owned. What this package
  canonicalises is the vocabulary a product builds its personality *out of* — a
  product differs by aliasing, not by editing.
- **Not a runtime.** Nothing here is a dependency. Serve `tokens.css` as a
  static file; it is inert CSS.
- **Not machine semantics.** Play-Nice owns status and state words. A human
  label here is a presentation mapping onto a Play-Nice meaning, never a new
  state — see [`docs/LANGUAGE.md`](../docs/LANGUAGE.md).

## Token vocabulary and naming

A token is `group/role`. In CSS the slash becomes a hyphen:

```text
surface/base           ->  --surface-base
status/needs_attention ->  --status-needs_attention
space/md               ->  --space-md
```

- **Names say what they mean.** `--border-line` is a border line. There is no
  `--brand-blue` and no `--grey-500`, and no `--space-13`.
- **The slash is the only structure.** `group` says what kind of thing it is
  (surface, text, border, action, focus, status, accent, space, radius, type);
  `role` says what it is for.
- **No prefix.** The names are unprefixed so they read the same in a token
  review as in a consumer's stylesheet. A consumer that also loads another token
  sheet aliases these into its own namespace — the way Studio does with
  `--studio-*`.
- **A structural role is a step on a named ladder, never a number.** `--space-lg`
  is a gap, and the ladder is capped, so there is always a largest one.
- **Literals stay in the token source.** A colour, length or shadow written
  directly into a product instead of read from a token is a decision that
  escaped review.

## The roles in this package

Two layers, and the difference between them is the whole design.

| Layer | Groups | Moves with the light? |
| --- | --- | --- |
| **structural** | `space/`, `radius/`, `type/` | no — a gap is a gap in every mode |
| **colour** | `surface/`, `text/`, `border/`, `action/`, `focus/`, `status/`, `accent/` | yes — that is what a mode is for |

### Structural roles: the vocabulary any UI needs

| Group | Roles |
| --- | --- |
| `space/` | `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl` — the gap ladder |
| `radius/` | `sm`, `md`, `lg`, `full` — corner rounding, `full` for a pill |
| `type/` | `micro`, `small`, `label`, `body`, `lead`, `h1` — the size ladder |

These three are the **structural** roles, and the reason they are here is worth
stating precisely, because it is a different reason from the one in #25.

Design-system#25's rule is that *a pattern* is promoted when **two or more**
products need it. That rule is about patterns, and it is unchanged. The first
package honoured it strictly and shipped colours only, which was right at the
time: one product consumed it, so promoting Studio's spacing values would have
made a single product's numbers into shared truth.

Structural roles are promoted on a different and stronger basis. **Spacing,
radii and a type scale are not product-specific. They are the vocabulary any UI
needs.** Every product on earth draws a gap, rounds a corner and picks a text
size, and those are not decisions a product should re-litigate privately. So
they are canonical here, reviewed once, consumed everywhere — on the grounds
that they are universal primitives, not because one product happens to use them.

That distinction is the whole argument, and it is the line that keeps personality
out:

| | Structural | Personality |
| --- | --- | --- |
| Examples | gap, corner, text size | a font family, a colour signature, a voice, an illustration style |
| Owner | this repo, canonically | the product that wears it |
| How a product differs | it may alias a structural role at a narrower scope | it simply has its own |

A product expresses personality by **aliasing**, not by editing: it may read
`--space-md` as something larger in its own layer, but the shared vocabulary and
its meaning stay intact. That is the same mechanism Studio already uses with its
`--studio-*` handles.

Three rules keep this honest:

- **No `px`.** Every length is `rem`. A pixel length is fixed against the
  person's own browser text size, and Play-Nice's accessibility contract does not
  let a UI win against that. A `rem` length grows with the reader.
- **No fluid sizes.** A `clamp()` couples the type ladder to one product's
  viewport decisions. That is layout, and layout is product-local. A product
  that wants fluid type does it in its own layer, aliasing these six.
- **A short, capped ladder.** Seven gaps, four radii, six sizes. Capping it is
  what stops the scale drifting into one-off values nobody else knows. A heading
  ladder past `h1` is a product's typographic decision, composed from these.

### Colour roles: meaning, and a mode per kind of light

| Group | Roles |
| --- | --- |
| `surface/` | `base`, `raised`, `sunken` — page, panel above it, inset below it |
| `text/` | `primary`, `soft` |
| `border/` | `line`, `line-strong` — the everyday line, and the one for a frame |
| `action/` | `primary`, `on-primary` — the one thing a person acts on |
| `focus/` | `ring` — the visible focus indicator |
| `status/` | one per Play-Nice status word, plus the human mappings |
| `accent/` | `mint`, `apricot`, `lilac`, `honey`; and teal's `tide`, `mermaid`, `iris`, `coral`, `kelp`, `sunbeam` (kept as aliases: tide = mint, kelp = done, mermaid = lilac, iris = partial, coral = failed, sunbeam = honey) |

Accent colours carry identity and emphasis. They are never the only signal:
status is carried in words, as Play-Nice requires.

#### The surface ladder is three, and that is deliberate

A retiring product kit drew a **six**-step surface ramp — void, canvas, hull,
panel, elevated, raised. This package has three. That is a considered collapse,
not a shortfall, and the reasoning is the naming rule above: the kit's six names
were six *rungs of one idea* (how far above the page is this), which is a
convenience for a component layer that may need any depth at all. Under
design-system#25 component CSS is product-local, so a product that wants a
deeper ramp builds it in its own layer from `--surface-raised`. Shared tokens
carry **roles, not rungs**.

The second consumer that raised the question is the honest test, and it answers
the same way. The generated gallery pages name **three** surface values, not
six: a page, a raised chrome/slot value, and a container value. They get their
fourth and fifth distinct jobs by *reusing* the raised value — a button hover
and an empty mosaic slot both read as the same raised surface. So the consumer
that looked like it needed the six-level distinction is drawing three roles and
one state, and the state is the part that is product-local by #25. What the
gallery actually wants from this repo is a **documented pattern** for its page —
cards, mosaic, tiles, tables, update list, empty state — not more surfaces.

What survives is the part that is genuinely a different **role**:

- `--surface-base` — the page everything rests on.
- `--surface-raised` — a panel lifted above the page. A header, a card, a dialog.
- `--surface-sunken` — a well pressed *into* the page. A dialog's backdrop, an
  inset code block, an empty slot. (A modal dialog reads correctly this way: the
  backdrop is the well and the dialog is the panel sitting in it.)

`hull` and `panel` therefore both land on `--surface-raised`, on purpose. If a
second product ever needs a genuinely distinct level — a menu or popover that
must read as *detached* rather than merely raised — that is a **missing token**
to be decided here, not a private literal in a product.

#### The border ladder is two, and the text ladder is two

`--border/line` is the everyday line; `--border/line-strong` is the one that
frames a thing. Two consumers independently draw that split — a hairline rule
between sections and a visible frame around a viewer are different jobs. A third
kit border, *hover*, is not a role: it is a **state**, and a state belongs to
the product's interaction. `line-strong` covers emphasis.

Text is `primary` and `soft`, and the second is deliberately one step not three.
A kit's *secondary* and *muted* are both "words that are not the main words".
Collapsing them means a dimmer paragraph is `text-soft` and not a third
vocabulary to keep in step. A placeholder for content that does not exist is a
**state** as well, so it reads `text-soft` too — and `text-soft` is measured
against every surface in the contrast report, so it stays readable.

### Status is two layers

`status/` carries the Play-Nice words themselves — `healthy`, `warning`,
`needs_attention`, `degraded`, `unavailable`, `not_configured`, `disabled`,
`stale`, `unknown`, `working`, `waiting`, `blocked`, `deferred`, `partial`,
`complete`, `failed` — so every machine meaning has a token. Six of those also
carry a human label: `status-needs-you`, `status-working`, `status-held`,
`status-done`, `status-failed`, `status-unknown`. Those are presentation
mappings, not new states, and `docs/LANGUAGE.md` says which label maps to which
word. `--status-needs-you` and `--status-needs_attention` are the same colour
because they mean the same thing.

## Modes

| Mode | When |
| --- | --- |
| Dark | the default, and `data-theme="dark"` |
| Light | the system asks for light and the person has not chosen |
| High contrast | `prefers-contrast: more`, and `data-theme="high-contrast"` |
| Low stimulation | only `data-theme="low-stimulation"` |

An explicit `data-theme` always wins over the system. Low stimulation never
switches on by itself, and it does not lower the floor.

## Pattern contracts

A pattern is promoted here when **two or more** products need it. One consumer
is not evidence of a shared need. A pattern is documented, never shipped: what
it must contain and which tokens it uses, so two products implementing it
separately still look the same.

Note what that rule governs: it governs *patterns*. It never governed structural
roles, which are promoted as universal vocabulary — see **Structural roles**
above. The two rules are different on purpose, and confusing them is how a token
set grows a product's numbers by accident.

A documented pattern states:

- **What it is for**, in one sentence, and what it is not for.
- **Which tokens it must use**, by name. A pattern needing a value no token
  carries has found a missing token, not a licence for a private literal.
- **The machine meaning it presents**, in Play-Nice words.
- **The floor it must meet**, by reference to Play-Nice — never by restating it.
- **At least one case it must handle honestly**: empty, loading, unknown, failed.

There are no pattern implementations here and no shared class names. The `wk-`
vocabulary is explicitly out of scope, not a migration target: it is one
product's implementation, and there is nothing to port.

## Icons, art and product chapters

- **Icons:** one style, the painted B Classic sticker set. Meanings, minimum sizes and the
  rules (always beside a word; status is a word; crew is presence) are in
  `assets/icon-set/vocabulary.json` and `assets/icon-set/README.md` in the design-system repo.
- **Art:** the media library and how it feeds the system: `docs/ART-SOURCES.md`.
- **The gallery** (the one gallery, gallery.hulganfamily.duckdns.org): `docs/STUDIO-GALLERY.md`
  (the sticker-board wall, project pages, icons, crew, motion), with the specimen
  `specimens/studio-gallery/`. The separate Studio web app is retired.

## The accessibility floor

**Play-Nice owns the floor. This repo references it and does not restate it.**

The canonical floor is the Play-Nice contract suite, pinned in this repo at
[`.project/contracts/adoption.yaml`](../.project/contracts/adoption.yaml): the
`accessibility`, `sensory-safety`, `web-ui` and `design-fidelity` surfaces, at
revision `1190e6a062c00b5932ade9c706d8ec1eea59775e` of
`Rylee-Bee/play-nice-contracts`.

One floor, one source. A product that needs a floor number in its own CSS reads
it from Play-Nice and says so; a second copy of the numbers is exactly how two
floors drift apart. So no file of this package states a target size, a contrast
ratio or a motion rule. A test fails if a pixel measurement appears in any of
them.

### The target minimum is a missing token, on purpose

The interactive-target floor is the sharpest case, because the package now
ships plenty of lengths and it would be easy to assume one of them is it. **It
is not, and there is no `--target-*` role here.** The floor is the third rule of
Play-Nice's `accessibility` contract at the pinned revision; this repo points at
it and holds no copy of the number. A product that needs a target size in its own
CSS reads it from Play-Nice, aliases it into a handle of its own naming — the
way Studio already does with `--studio-target-min` — and says where it came from.

A gap, a corner and a text size are not the floor. Shipping them here is the
owner's decision about shared vocabulary; shipping the floor here would be a
second floor, and that is the thing this section exists to prevent.

What the package *does* take from the floor is its **direction**, and that is a
unit, not a number. The same contract refuses to let a UI block browser zoom and
text scaling, so every length here is `rem` and scales with the reader. That is
why the widening did not weaken the pixel-literal test: the rule is not "this
package has no lengths", it is "this package never states a floor".

The contrast evidence for the one thing this repo does own — the colour values —
is the report at [`tokens/CONTRAST.md`](../tokens/CONTRAST.md), and that is
measured, not declared.

## System identity

| Field | Value |
| --- | --- |
| Name | Rylee-Bee Design System |
| Repo | `Rylee-Bee/design-system` |
| Path in repo | `generator` |
| Source id | `estate` |
| OpenDesign system | `user:rylee-bee-studio` (shown as "Rylee Bee Studio") |
| Category / surface | Shared & Systematic / web |

These are the strings a consumer's registry declares; Studio's copy is
`tools/design-sources.json`, and both repos assert the same values. This is the
estate's shared identity, not a product's: nothing here is Personal World.

## Provenance

`tokens.css` is generated from `tokens/mulberry-mint.tokens.json` (the Mulberry &
Mint base from the World Tree Field Guide, owner's pick 2026-10-12) by
`tokens/build_package.py`. Edit the token source, run the builder, commit both.
The build is byte-stable, so a diff after a rebuild means the two disagree.
See [`README.md`](README.md) for the commands, the stamp, and the decisions.
