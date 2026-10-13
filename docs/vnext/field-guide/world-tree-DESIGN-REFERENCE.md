# World Tree Field Guide design reference

The **World Tree Field Guide** is the current Figma exploration surface for making the estate's World Tree model easier to understand.

**Figma:** https://www.figma.com/design/bDhXQc5xZRgru2geXrBDjq

## Authority boundary

This file is **design evidence, not canonical estate truth**.

Authority remains:

1. purpose and cooperation contracts,
2. merged repository decisions and machine-readable estate truth,
3. CI and other execution evidence,
4. runtime observation,
5. human-facing reconciliation and explanation.

The Field Guide may simplify, illustrate, and teach those layers. It must not silently invent authority, relationships, machine identities, or runtime facts.

Shared visual tokens and reviewed design memory remain owned by `rylee-bee-labs/design-system`. The Figma Field Guide consumes the published Rylee-Bee design-system variables; Figma itself does not outrank the repository.

## Canonical product model

The Field Guide should now explain the product through the same small skeleton as the repository:

- **Home** — orientation: what matters right now?
- **Explore** — exploration and synthesis; currently may be staffed by Hive Works.
- **Needs You** — consequential human decisions; authority remains with the human through Project Home's supported boundary.
- **Guide** — understanding and continuity; currently may be staffed by Ant Works.

The visual model has three layers:

1. **Platform** — stable contracts and evidence.
2. **Capabilities** — replaceable implementations of stable jobs.
3. **Experience** — replaceable themes, staff, characters, metaphor, voice, and environments.

**Worlds belongs to Experience.** It is a theme/package and a design playground over the skeleton, not one of the skeleton's permanent structural roles.

Field Guide reviews should distinguish:

- **new capability** — may change architecture and needs evidence,
- **new worker** — must satisfy an existing capability contract,
- **new costume** — presentation may change freely while semantics remain stable.

## Current design specimens

The exploration currently includes:

- **Cover + Source** — “Stable meaning. Mutable form.” and the source/design-system relationship.
- **The Tree** — stable responsibilities first; ecological labels are optional explanatory costumes, not fixed implementation mappings.
- **The Rings** — architectural memory for meaningful changes, including “Can I forget this now?”
- **Growing + Pruning** — try locally → observe → repeat → prove → promote → migrate + prune.
- **Anatomy / The Skeleton** — purpose, stable identity, owner, truth, authority, relationships, evidence, replaceability, lifecycle, unknowns, recovery, explanation.
- **Connections + Rename Radius** — evidence-backed relationships, explicit UNKNOWN handling, and a rename blast-radius ladder from display names through runtime identity.

The visual language, staffing, and ecological metaphor may change. The four-door skeleton and capability meanings change only deliberately.

## Connections specimen

The Figma connections view is intentionally conservative:

- an observed or repository-backed edge may be shown as evidence,
- no search hit is **not** proof that no relationship exists,
- runtime-only dependencies require runtime evidence,
- a relationship does not grant authority by itself,
- soft hints remain soft until behavior is verified.

When the Field Guide and repository truth disagree, fix the Field Guide or mark the question UNKNOWN. Do not “correct” repository truth merely to match the drawing.

## Rename safety

The current design expresses four increasing blast-radius levels:

| Level | Guidance |
| --- | --- |
| Display | Change freely when meaning is preserved. |
| Repository | Sweep references, remotes, docs, badges, paths, and automation references. |
| Contract / config | Migrate consumers of IDs, manifests, inventory keys, workflow callers, aliases, APIs, and configuration names. |
| Runtime | Observe first: DNS, routes, service names, volumes, secret references, and deploy targets may carry operational identity. |

Safe rename sequence:

> **meaning → references → compatibility → migration → proof → prune → ring**

A rename is not complete merely because the new name works. The old path must either be deliberately retained as compatibility or retired with evidence.

## Review rule

Use the Field Guide to answer the human question first:

> What job is this responsible for?

Then expose the engineering truth underneath:

- Where is canonical truth?
- Who may change it?
- What does it depend on or serve?
- What evidence proves what happened?
- What survives if the current implementation or name disappears?
- What is still UNKNOWN?
- How do we recover from a mistake?

If a future design adds a new concept, first ask whether it represents a genuinely new responsibility or only a new costume for an existing one.


## Signal rule

> Quiet when things are working. Precise when they're waiting. Loud only when something genuinely needs attention.
> — Rylee, 2026-10-08

The principle under it, in her words: *the estate should distinguish actual problems from expected waiting, without
making Rylee investigate either one.* It is applied to the truth Project Home already presents: no new dashboard or service.

Two cases found on 2026-10-08, each fixed by making the record more precise rather than louder:

- **A new notification category must not silently gain the right to push.** Every category declares its own push
  default, an undeclared or unsaved category does not push, and a test fails when one is added without a default
  (project-home#160). A new category arriving was normal behaviour; buzzing her phone for it was not.
- **A mission deliberately waiting for a repository is not stale.** Its health says what it waits for and who holds the
  lock (agent-platform#224).

**Waiting and staleness are separate fields.** A mission can be waiting on purpose while its own evidence goes stale.
Show both, and never let a reassuring *waiting* label hide a genuine *stale evidence* problem (Rylee, 2026-10-08).

For any new signal, ask: is this working (stay quiet), waiting (say on what, and who holds it), or a genuine problem
that needs a person (be loud, once)? An unknown or unclassified case is shown as UNKNOWN, never as fine.

## Product journey specimen: Explore / Candy Dispenser

The Field Guide now includes a theme-neutral Explore journey that proves the stable product skeleton can host parallel exploratory work without exposing orchestration as the product.

Human journey:

```text
question
  → Explore
  → Hive Works fans out
  → Lab CLI structures bounded work and evidence
  → progress + evidence + dissent + UNKNOWN return
  → synthesis
  → useful output
  → optional proposal to Needs You
```

Product rules:

- The human sees the **shape and result of work**, not agent exhaust.
- Workers are replaceable. The question, evidence, and useful synthesis survive them.
- Lab CLI belongs to **Hill Works**, not Hive Works. Hive may use Hill Works tunnels, but does not own them.
- Hill Works provides bounded, repeatable routes through machinery; Lab CLI exposes those routes through a disciplined interface.
- Explore may research, compare, map, critique, draft, and propose.
- Explore does **not** gain approval authority from parallelism or personality.
- A consequential proposal crosses into **Needs You** with its evidence attached.
- Project Home remains the human authority boundary underneath that decision.
- An exploration may end without becoming durable architecture.
- Useful outputs may become an artifact, enter Lore review, or become a proposal.
- Explicit UNKNOWN is a successful outcome when evidence is insufficient.

The Candy Dispenser is a replaceable experience metaphor for this flow. The capability is **explore and synthesize**; the current staffing may be Hive Works; the costume may change.

Review question:

> If every bee, candy metaphor, and agent name disappeared tomorrow, would the flow still be obvious as **ask → investigate → synthesize → optionally decide**?

The answer must be yes.


## Product journey specimen: Guide / Ant Works

Guide proves the continuity half of the stable product skeleton. It answers from existing durable evidence rather than turning every question into fresh exploration.

Human journey:

```text
question in remembered language
  → plain answer
  → evidence trail on demand
  → engineering truth on demand
  → relationships + history
  → change impact
  → explicit UNKNOWN at evidence boundary
  → memory permission
  → optional proposal to Needs You
```

Product rules:

- **Answer plainly first.** Progressive disclosure exists so the user does not have to ingest the estate graph to get one answer.
- Ant Works may trace, map, explain, carry receipts, and maintain paths. It is replaceable staffing for Guide, not authority.
- A claim about **responsibility or canonical truth** and a claim about **current runtime state** require different evidence. Repository truth must not masquerade as live observation.
- A missing search hit is not proof that no relationship exists.
- When evidence ends, Guide returns **◌ UNKNOWN** and may explain what observation could resolve it.
- Story, relationships, engineering truth, evidence, and history are progressively disclosed rather than flattened into one technical page.
- Tree Rings exist to preserve why a meaningful change happened and whether the old mental model can be forgotten.
- Guide may preview blast radius and explain migration shape, but consequential action becomes a proposal to **Needs You**.
- Guide succeeds when future-Rylee can recover truth while remembering fewer implementation details.

The durable capability is **understand and recover continuity**. Ant Works is current staffing; ants are not load-bearing architecture.

Review question:

> If Ant Works vanished tomorrow, could another implementation still answer **what owns this, why, what touches it, what changed, how do we know, and where does certainty end**?

The answer must be yes.


## Trail-to-tunnel model

The current ecology separates three related jobs:

| System | Primary job | Human-facing artifact |
| --- | --- | --- |
| **Hive Works** | discover, gather, compare, refine, synthesize | **Candy Dispenser** |
| **Ant Works** | trace, explore when needed, establish evidence-backed followable trails | **Field Guide** |
| **Hill Works** | operationalize repeated, proven routes as reliable maintained tunnels | **Lab CLI** |

An Ant Works explorer may enter uncertain territory, but its useful output is a route another human or worker can follow. The Field Guide is the human-readable projection of those established trails.

Hill Works should not pave every path. A route earns a tunnel through repeated, proven use.

```text
discovery
  → evidence-backed ant trail
  → Field Guide
  → repeated use
  → proven value
  → Hill Works tunnel
  → Lab CLI
```

This model lets Hive Works, Ant Works, Needs You, CI, or other bounded workers use the same maintained tunnels without making Hill Works part of their identity.


## Worlds interface direction: Mulberry & Mint

The Figma Field Guide has a separate, product-specific interface exploration selected by Rylee. It is **PLAY design evidence**, not a promotion of Worlds' theme, card composition, or prototype data into shared estate canon.

- **Chosen composition:** Launchpad Home paired with Split View inspection.
- **Chosen palette:** Mulberry & Mint — deep plum canvas, lighter plum surfaces, mint active/selected states, soft lilac text and borders, and apricot evidence emphasis.
- **Shared journey cue:** Home → Thread → Evidence → Companion → Project Home. The highlighted step shows where the prototype is; it does not assert a runtime event or authority transition.
- **Card detail control:** Learner / Technical / QA / Agent. The four readings alter explanation only. The underlying record and permissions stay the same.
- **Experience path:** Home → selected thread → inspect evidence → ask Companion for explanation → open the governed Project Home decision flow.

**Figma design:** https://www.figma.com/design/bDhXQc5xZRgru2geXrBDjq/World-Tree-Field-Guide?node-id=79-1395

**Clickable prototype:** https://www.figma.com/proto/bDhXQc5xZRgru2geXrBDjq/World-Tree-Field-Guide?node-id=85-1926&starting-point-node-id=85-1926

The prototype has four Home and four inspection examples for the reading tiers, plus evidence, scripted Companion, and Project Home handoff screens. It uses illustrative example data. The conversation is scripted; it does not connect to Companion or read live Project Home state. Tier choices are demonstrated by linked screen states; independent per-card override and remembered-preference persistence remain future design/implementation work.

For future review, keep the authority boundary visible: Companion may explain; evidence must remain inspectable; missing receipts remain UNKNOWN; consequential owner decisions continue in Project Home. Preserve the shared four doors (Home, Explore, Needs You, Guide) and the experience-layer status of Worlds.
