# Packet: worlds-vnext / a-research

## Objective
Write `docs/vnext/design-research.md`: a design-research note that (1) indexes the local design references and (2) extracts the *mechanics* (interaction patterns, not colour/skins) of five reactive-UI references — Homarr 2.0, Appsmith, Dashy, Home Assistant custom cards, MCP ext-apps — as reusable building blocks for a 30-concept exploration of Worlds. Commit it on your clone; do not push.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` cut from `origin/main` (786f506). Work and commit ONLY in your clone; never touch the real checkout, never push.

Read-only inputs outside the repo — read them if the path exists on this host, otherwise write `UNKNOWN` for that input rather than inventing anything:
- Local design index: `~/code/Rylee-Bee/rylee-bee-workspace/media_files/designs/_index` (list it; summarise the kinds of references it holds).
- The five references named above. If you have no network and no local copy, use only what you can state with confidence and mark the rest `UNKNOWN`.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee comment 6153184069 on #322: the three draft directions "too unimaginative"; a wider 30-concept exploration with working interactions before choosing.
- 2026-10-13 brief.md update 19:20Z: "Static images are not prototypes."
- 2026-10-13 brief.md update 19:20Z: each concept needs "three working interactions with one updating several views, an embedded demo tool plus fallback, agent-visible shared facts with separate permissions, honest provenance/stale/UNKNOWN, working phone/keyboard variant".
- 2026-10-13 brief.md Authority: protected invariants — local Memory, accessibility floors, zero-provider usability, security/consent, no invented status, external systems keep their own authority.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY survey, then write ONLY `docs/vnext/design-research.md` (and commit it). It must contain:
  1. An index of the local design references (what exists, where, what each is good for), or `UNKNOWN` where unreadable.
  2. One section per reference (Homarr 2.0, Appsmith, Dashy, Home Assistant custom cards, MCP ext-apps) with the *mechanics* worth stealing: how state is surfaced, how a user acts, how staleness/unknown is shown, how phone/keyboard work — with a source link or `UNKNOWN`.
  3. A short list (8-15) of concrete reusable interaction patterns the concept batches can build on.
- out: any file other than `docs/vnext/design-research.md`; any code change; any push; any claim presented as fact that you could not verify (mark it `UNKNOWN`).

## Acceptance
`test -s docs/vnext/design-research.md && grep -qiE 'mechanics' docs/vnext/design-research.md && grep -qiE 'unknown' docs/vnext/design-research.md`

## Evidence
`docs/vnext/design-research.md` — the five reference sections each name at least one mechanic; unreadable inputs are marked `UNKNOWN`, not omitted silently.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
