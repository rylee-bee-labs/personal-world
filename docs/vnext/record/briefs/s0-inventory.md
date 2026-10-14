# Packet: worlds-vnext / s0-inventory

## Objective
Write `docs/vnext/inventory.md`: a source-linked ownership + consumer inventory of the current Worlds code with a keep/adapt/replace/retire/unknown call per module (#312, #319).

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Work and commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design." then "Ok do add the api and visuals. Hah" — cleanup runs first; design and contract work run beside it.
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks"
- 2026-10-13 issue #319: "Classify current implementation, with source-linked evidence" into FRONT_DOOR_CORE / EXPLICIT CORE EXCEPTION / PROVIDER_ADAPTER / EXTERNAL_AUTHORITY / LEGACY, and "name live consumers and preservation tests before changes."
- 2026-10-13 issue #321: "The current implementation is evidence and optional reusable code, not a mandatory design or API structure." and "Preserve legacy code until dependency/rollback evidence allows removal."
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY survey of `src/personal_world/worlds/*`, `src/personal_world/providers/*`, `ui/src/fd/*`, `ui/src/generated/*`, `tests/`, route registrations and their importers/consumers; then write ONLY `docs/vnext/inventory.md` (and commit it).
- out: any edit to `src/`, `ui/`, `tests/`, config, or existing docs; any deletion; any new connector contract; any rebake of the dashboard.

## Acceptance
`test -s docs/vnext/inventory.md && grep -q 'personal_world/worlds' docs/vnext/inventory.md && grep -qi 'consumer' docs/vnext/inventory.md && [ "$(grep -ciE 'keep|adapt|replace|retire|unknown' docs/vnext/inventory.md)" -ge 3 ]`

## Evidence
`docs/vnext/inventory.md` — every row cites a `path:line` and its live consumer (grep/import evidence), plus a keep/adapt/replace/retire/unknown call.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
