# The World Tree

The **World Tree** is the human/ecological map of the Rylee-Bee estate.

It is not a new orchestrator, service, repository, authority layer, machine vocabulary, or control plane. It is a second coordinate system for understanding how independently useful systems relate, what role they play, and how the estate grows.

> **The World Tree describes how the estate relates. It does not replace what the estate calls things.**

## Two identities, one truth

An estate component may have both:

| Identity | Purpose |
| --- | --- |
| **Canonical identity** | Architecture, authority, contracts, APIs, repositories, decisions, machine state |
| **Ecological identity** | Orientation, storytelling, visualization, product character, systems reasoning |

The ecological identity must preserve the canonical one. It may make meaning easier to see, but it cannot create new machine semantics, move authority, hide provenance, or silently rename an implementation.

This follows the estate's existing language boundary: machine truth first, then plain human wording, then optional product/character voice.

## Product skeleton

The human-facing product keeps a deliberately small vocabulary:

| Door | Human question | Stable capability |
| --- | --- | --- |
| **Home** | What matters right now? | orientation |
| **Explore** | What can we discover or make? | exploration and synthesis |
| **Needs You** | What requires me? | human authority |
| **Guide** | How does this work, and why? | understanding and continuity |

These four doors are stable product structure. Their implementations, staff, metaphors, characters, and visual treatment are replaceable.

Current examples include **Hive Works** staffing Explore and **Ant Works** staffing Guide. The **Candy Dispenser** is Hive Works' human-facing synthesis metaphor. **Hill Works** is shared work machinery that turns proven routes into reliable, reusable tunnels; the **Lab CLI** is its structured interface to those tunnels. None of these are additional top-level product areas.

Project Home remains the authority machinery beneath consequential Needs You actions.

### Three layers

1. **Platform** — contracts, identity, evidence, state, provenance, accessibility, recovery, and authority boundaries. Intentionally conservative.
2. **Capabilities** — replaceable implementations that satisfy stable jobs such as orientation, exploration, authority, continuity, search, and future proven needs.
3. **Experience** — themes, characters, metaphor, staffing, voice, environments, and other presentation. Intentionally playful and replaceable.

**Worlds is an experience/theme package, not a structural product role.** It may dress the four doors and provide a place to experiment with interaction and personality without becoming infrastructure.

> **Sameness is not coherence.** Coherence comes from shared contracts, understandable boundaries, durable meaning, accessibility, and evidence.

Whimsy may rename, recast, decorate, narrate, and inhabit a capability. It may not silently redefine that capability.

When an experience experiment discovers something broadly useful, promote only through evidence:

> **try → observe → repeat → prove → promote → migrate → prune**

Most good local ideas do not need promotion.

A useful review question for every new idea is:

> **Is this a new capability, a new worker, or a new costume?**

New capabilities deserve architectural scrutiny. New workers satisfy an existing contract. New costumes are free to play.

## Existing authority language remains canon

The World Tree does not replace the estate shorthand:

> **Hive thinks. Project Home commits. Companion converses. Worlds presents. Lore remembers. Hermes executes within granted bounds.**

In particular:

- Project Home remains the canonical human authority boundary for consequential estate actions.
- Hive Works explores and proposes. It does not gain approval authority because it is represented as a hive.
- A Worlds theme may present estate state. Calling a surface the canopy does not make the theme canonical for that state.
- Lore remains reviewed durable canon. Calling it tree rings does not turn every log or runtime record into Lore.
- External systems do not become estate-owned because an integration is drawn as a vine.

## Ecological vocabulary

World Tree language is a **replaceable explanatory vocabulary**, not a fixed mapping from metaphor to product.

| Ecological idea | Useful meaning | Current example, when useful |
| --- | --- | --- |
| 🌳 **World Tree** | the estate as a connected whole | topology, relationship, belonging, growth |
| 🐝 **Hives / bees** | exploration, parallel gathering, refinement and synthesis | Hive Works may staff **Explore** |
| 🍯 **Honey / sweets** | useful synthesized output brought back from exploration | Candy Dispenser framing around Hive work |
| 🐜 **Ants** | tracing, exploring when needed, carrying evidence, and leaving followable trails | Ant Works may staff **Guide**; established trails feed the **Field Guide** |
| ⛏️ **Hills / tunnels** | operationalizing repeated, proven routes into maintained paths | Hill Works; **Lab CLI** exposes usable tunnels |
| 🐦 **Birds** | expressive experiences that can come and go | games and experiments |
| 🌰 **Acorns** | portable outputs | things useful away from Rylee's exact estate |
| 🪵 **Rings** | durable explanation of meaningful change | reviewed architectural memory |
| 🌱 **Roots** | durable foundations | mechanisms deeper than any one experience |
| 🌿 **Vines** | replaceable external relationships | connected-service integrations |

A theme package may add a canopy, station, cottage, ocean, cartoon command center, or something not yet invented. **Worlds is one such experience/theme package.** None of those presentation choices becomes a stable product role merely because it is delightful.

The ecology is intentionally incomplete. A component does not need an animal, plant, room, or mythic role merely because one is available.

The ecology is also **not a mandatory visual theme**. Products and theme packages keep their own character. A surface may use no tree imagery at all and still satisfy the same product skeleton.

## Architectural test

An ecological term is useful only when it helps answer a real architecture question.

Good examples:

- Bees can be ephemeral because the useful synthesis can survive the worker.
- Ants should have bounded jobs, permissions, stores, and territories.
- A vine can be removed without pretending the external provider was part of the estate.
- An acorn should be useful away from Rylee's exact environment.
- A bird may disappear without becoming a structural failure of the Tree.

Bad example:

> "This repo needs an animal because the other repos have one."

If the metaphor stops teaching us something, keep the canonical term and leave the ecological niche unclaimed.

## Where related material lives

- [MAP.md](MAP.md) maps the current ecology.
- [SUMMARY.md](SUMMARY.md) is one row per repository: inbound edges, the top
  callers, and the UNKNOWN count — followed by what the run that produced it
  could not see (paths skipped, files over 2 MB, files unreadable, repositories
  not checked out). It is the committed read of
  [`build_graph.py`](build_graph.py); the full `graph.json` and
  `BLAST-RADIUS.md` (~7MB, ~510k lines) are generated on demand into a
  gitignored directory and are not committed.
- [GLOSSARY.md](GLOSSARY.md) defines terms and protects known collisions.
- [RENAMES.md](RENAMES.md) separates naming experiments from canonical renames.
- [DESIGN-REFERENCE.md](DESIGN-REFERENCE.md) records the current Figma Field Guide as design evidence, including its authority boundary, connection rules, and rename safety model.
- Estate-wide authority decisions remain in [../decisions/README.md](../decisions/README.md).
- The vocabulary census remains discovery-only in [../estate-language/rylee-estate-language.md](../estate-language/rylee-estate-language.md).
- Shared visual treatment belongs in the `rylee-bee-labs/design-system` repository.
- Durable shared media bytes follow the estate's media ownership/custody decisions.

## World Tree principle

> **Build an ecosystem whose inhabitants can change without losing the world they belong to.**


## Trails, Field Guide, and tunnels

The ecology distinguishes **discovering**, **making understandable**, and **operationalizing**:

> **Hive discovers → ants establish followable trails → the Field Guide preserves those trails → repeated and proven trails may become Hill Works tunnels → Lab CLI exposes those tunnels cleanly.**

Some ants are explorers. Their defining contribution is not novelty alone: **when an ant goes somewhere, it should leave the route easier to understand and follow than it found it.**

The **Field Guide** is the human-readable projection of evidence-backed trails established by Ant Works. It is not canonical truth, another database, or another authority layer.

Hill Works follows behind when a trail has earned operationalization. It builds and maintains bounded, reusable routes through estate machinery. The Lab CLI is Hill Works' structured human/agent interface to those proven routes.

Not every trail becomes a tunnel. A useful footpath may remain a footpath forever.

This is the same promotion discipline expressed ecologically:

> **discover → understand → repeat → prove → operationalize → migrate → prune**

and remains compatible with:

> **try → observe → repeat → prove → promote → migrate → prune**


## Forget safely

The estate should reduce the amount of implementation detail Rylee must keep in working memory.

> **Help Rylee forget implementation details safely. Do not help her forget unresolved contradictions.**

Agents and humans extending the World Tree should re-ground in current evidence before completing a pleasing pattern.

Before promoting an idea, check:

1. **Truth** — what do the owning repositories and current evidence say?
2. **Meaning** — what stable responsibility is actually changing?
3. **Evidence** — was this observed, or are we completing a pattern because it looks tidy?
4. **Existing capability** — does something already do this job?
5. **Promotion** — has repeated use earned permanence?
6. **Retirement** — if this replaces something, what old path eventually disappears?
7. **Authority** — does the change accidentally move human authority?
8. **Forget test** — can Rylee stop remembering the implementation after the change?

Use three maturity labels during design:

- **PLAY** — a useful or delightful idea being explored.
- **PROVING** — repeated usefulness is being tested and evidence gathered.
- **CANON** — responsibility, ownership, evidence, migration, and boundaries are recorded.

Prefer contradiction over completion: when evidence conflicts with a neat model, surface and resolve the contradiction rather than filling the model's empty slot.

The acceptance test is:

> **Could Rylee return after forgetting how this was built, recover the truth from evidence, and safely continue?**
