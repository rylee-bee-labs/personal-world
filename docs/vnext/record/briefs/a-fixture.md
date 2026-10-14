# Packet: worlds-vnext / a-fixture

## Objective
Create the two shared inputs every concept in the 30-concept Worlds exploration will use: `docs/vnext/fixture/fixture.json` (one constant, synthetic data fixture — the same services, states, facts and permissions for all concepts) and `docs/vnext/fixture/concept-brief.md` (the template each concept brief is cut from). Commit both on your clone; do not push.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world`. Work and commit ONLY in your clone; never touch the real checkout, never push.

Inputs to read if present on this host (mark anything you cannot read `UNKNOWN` rather than inventing it):
- `docs/vnext/design-research.md` — the design-research note (14 reusable interaction patterns). Read it if the file is present.
- The house design library, by ABSOLUTE path: `/var/home/agent/code/Rylee-Bee/rylee-bee-workspace/media_files/designs/_index/` — especially `INDEX.md`, `DESIGN-QUALITY-GUIDE.md`, `PERFECT-PROMPT-TEMPLATE.md`, `MORNING-README.md`. (Use the absolute path, not `~/`: the worker's HOME is not the estate home, so `~/code/...` does not resolve.)

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee comment 6153184069 on #322: the three draft directions "too unimaginative"; a wider 30-concept exploration with working interactions before choosing.
- 2026-10-13 brief.md update 19:20Z: "Static images are not prototypes."
- 2026-10-13 brief.md update 19:20Z: each concept needs "three working interactions with one updating several views, an embedded demo tool plus fallback, agent-visible shared facts with separate permissions, honest provenance/stale/UNKNOWN, working phone/keyboard variant".
- 2026-10-13 brief.md Authority: protected invariants — local Memory, accessibility floors, zero-provider usability, security/consent, no invented status, external systems keep their own authority.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: `docs/vnext/fixture/fixture.json` and `docs/vnext/fixture/concept-brief.md` only.
  - `fixture.json`: ONE constant fixture — a small set of synthetic services (at least: one healthy, one needing a human, one stale, one unknown/disconnected), each with the fields a concept needs to render state: name, kind, current state, freshness/provenance, the facts it exposes (with per-fact agent/human permission), and its owning app + fallback link. Include a couple of failed/retry-able items and one item with zero data. Everything synthetic; no real secrets, hosts, tokens or personal data. Valid JSON (`jq .` must parse it).
  - `concept-brief.md`: the template — the headings and required sections every one of the 30 concept briefs will fill (concept name and one-line idea; the three working interactions; the embedded demo tool + fallback; agent-visible shared facts and separate permissions; how stale/unknown/provenance is shown; phone + keyboard behaviour; MOCKUP labelling; what is invented vs measured). Draw its quality bar from the design library's `DESIGN-QUALITY-GUIDE.md` / `PERFECT-PROMPT-TEMPLATE.md` if readable.
- out: any other file; any code change; any push; any real secret or personal data; any claim about the library or the research note presented as fact if you could not read it (mark `UNKNOWN`).

## Acceptance
`test -s docs/vnext/fixture/fixture.json && test -s docs/vnext/fixture/concept-brief.md`

## Evidence
Both files, with `jq . docs/vnext/fixture/fixture.json` parsing cleanly and the template listing every heading the brief requires.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
