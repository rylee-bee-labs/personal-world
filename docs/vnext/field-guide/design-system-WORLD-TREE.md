# World Tree presentation grammar

**Status: candidate design-family grammar, not a contract or machine vocabulary.**

The canonical ecological meaning lives in
`Rylee-Bee/rylee-bee-workspace/docs/world-tree/`.

This document owns only **presentation guidance**: how the estate ecology may be
expressed visually without moving authority, inventing state, or forcing every
product to wear the same costume.

## The rule

> **The estate root owns what the ecology means. The design system records how
> the ecology may look and feel. Products own their own implementation and
> personality.**

If the ecological presentation disappeared tomorrow, the product must remain
precise, operable, and understandable.

## One truth, four readings

This is a presentation pattern built on Play-Nice
`depth-on-demand` and `one-truth-two-views`. It does **not** add a new
machine status, schema, authority layer, or audience-specific source of truth.

An important estate object should be understandable at four useful readings
without changing what the object means:

| Reading | Useful for | It should answer |
| --- | --- | --- |
| **Plain** | a smart newcomer, including a nine-year-old | What is this? Is it okay? Do I need to do anything? |
| **Technical** | an operator or technical person | What happened, why, what owns it, and what depends on it? |
| **Provenance / QA** | an AI engineer or reviewer | Which model/agent/tooling acted, against which inputs, under which gates, and what evidence supports the result? |
| **Machine** | an AI agent or automation | Stable IDs, states, timestamps, SHAs, relationships, evidence links, allowed actions, and schema version. |

The first three are human depths. They follow Play-Nice's ladder from glance
through technical and specialist detail. The machine reading is the parallel
structured form required by `one-truth-two-views`; it is generated from the
same record, never scraped back out of the prose.

A surface may present these as a remembered Simple/Technical preference,
progressive disclosure, a provenance drawer, or an explicit machine view. The
control is product-local. The invariant is shared:

> **One object. One truth. More detail when asked. No translation may create a
> different fact.**

For cards and live indexes, preserve the same underlying fields as depth grows:
identity, state, reason, owner, evidence, action/attention requirement,
provenance, and visible UNKNOWN where evidence is incomplete. A shallow view may
omit detail; it may not contradict, invent, or silently upgrade it.

## Family resemblance, not matching costumes

The World Tree is not a mandatory forest skin.

A product can belong to the ecology without drawing a single leaf. Ecological
imagery is earned when it explains a relationship, makes provenance easier to
see, makes a place easier to understand, or adds delight without hiding truth.

Do not add:

- bark textures to unrelated controls,
- hexagons merely because Hive Works has bees,
- ants to every security screen,
- bird imagery to every game surface,
- green as an unlabelled semantic status,
- decorative ecology that makes the underlying state harder to read.

The existing design thesis still wins: **family resemblance matters more than
matching costumes.**

## Shared ecological motifs

| Motif | Presentation meaning | Useful places |
| --- | --- | --- |
| World Tree | connection, belonging, estate-scale orientation | maps, overview art, onboarding |
| Canopy | inhabitable human-facing experience | Worlds illustrations, navigation/orientation |
| Hive / bees | exploration, parallel curiosity, synthesis | Hive Works identity and work visualization |
| Honey | gathered material made useful | synthesis/review moments, exports |
| Hill / ants | structured work, continuity, guarded operations | Project Home/Hill Works identity and operational visualization |
| Birds | expressive games and experiences | estate maps, launchers, celebration art |
| Acorns | portable possibility | export/package/reference-implementation moments |
| Rings | durable reviewed history | Lore orientation and archival imagery |
| Roots | foundational mechanisms | infrastructure maps, dependency explanations |
| Vines | integration seams to external services | connector maps, external dependency explanations |

These are human presentation mappings only. They are never parsed back into
machine state.

## Ownership

| Layer | Owner |
| --- | --- |
| Ecological meaning and architecture guardrails | `rylee-bee-workspace/docs/world-tree/` |
| Shared presentation memory and cross-product visual grammar | this repository |
| Hive Works honeycomb interaction pattern | Hive Works |
| Hill Works/Project Home operational evidence pattern | Project Home |
| Product-specific component implementation | owning product |
| Durable media bytes | estate media ownership/custody path |

The reference specimens below live here because this repository keeps reusable
design memory. They do **not** promote one-product patterns into shared estate
patterns.

- [Hive Works honeycomb specimen](design-memory/world-tree/HIVEWORKS-HONEYCOMB.md)
- [Hill Works evidence-chain specimen](design-memory/world-tree/HILLWORKS-EVIDENCE-CHAINS.md)

## Accessibility and truth

Ecological presentation must obey the same floor as every other design:

- meaning is never carried by color or shape alone,
- every illustrated state has a readable textual equivalent,
- dense visual maps have a list/table view,
- focus order follows the actual work order rather than decorative geometry,
- zoom/reflow must not require understanding the original spatial layout,
- reduced-motion users lose no meaning,
- provenance and review state stay inspectable,
- UNKNOWN stays visible when evidence is incomplete.

## Design test

Before adding an ecological motif, ask:

1. **What relationship does this help a person understand?**
2. **What canonical fact is underneath it?**
3. **Can the person ignore the artwork and still do the job?**
4. **Does the product still feel like itself rather than a themed clone?**
5. **Would a missing fact remain visibly UNKNOWN instead of being papered over by the metaphor?**

If the motif cannot answer those questions, it is decoration rather than design
memory.


## Worlds interface study: Mulberry & Mint

**Provenance:** Rylee-selected Figma exploration, 2026-10-08. This is a product-specific reference specimen in PLAY; it is not a shared theme, token promotion, or component contract.

**Update 2026-10-12:** Rylee promoted this palette to the shared base. The tokens now live in `tokens/mulberry-mint.tokens.json` (see `.project/DECISIONS.md`, "The Field Guide is the base"). The rest of this section is the 2026-10-08 study as it was written.

The selected pairing combines a welcoming **Launchpad Home** with a **Split View** for thread inspection, evidence, and Companion context. The chosen product palette is deep mulberry/plum with mint selected states, light lilac text and outlines, and apricot for evidence emphasis. Figma palette values shown in this study are local to the Worlds exploration and are not additions to this repository's canonical token package.

The repeated progress path—**Home → Thread → Evidence → Companion → Project Home**—gives the prototype a consistent visual spine. A mint marker identifies the current prototype screen; it does not imply a runtime fact or authorization.

The four card readings remain **Learner / Technical / QA / Agent**. They change the explanation shown, not record identity, evidence, or permission. The mockups preserve the product boundary: Companion explains; Project Home owns consequential decisions.

- [Figma color comparison](https://www.figma.com/design/bDhXQc5xZRgru2geXrBDjq/World-Tree-Field-Guide?node-id=79-1395)
- [Clickable Home-to-evidence prototype](https://www.figma.com/proto/bDhXQc5xZRgru2geXrBDjq/World-Tree-Field-Guide?node-id=85-1926&starting-point-node-id=85-1926)

This is editable Figma UI with example data, scripted Companion copy, and prototype navigation. It does not connect to live data or implement independent per-card preference persistence. Treat it as D2 composition with selected D3 click-through behavior, not as product implementation or D4 production experience evidence.
