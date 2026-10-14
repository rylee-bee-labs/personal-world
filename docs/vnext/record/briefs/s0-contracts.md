# Packet: worlds-vnext / s0-contracts

## Objective
Audit API-type duplication (handwritten vs generated) and prove whether the OpenAPI/client drift checks are actually consumed (#317). Deliverable `docs/vnext/contracts-audit.md`.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 issue #317: "This is a potential double source of truth, not evidence that API shapes are currently wrong." Run existing generation/drift gates "without modifying generated artifacts".
- 2026-10-13 issue #317: "Recommend the smallest deletion/refactor slices; implement only after #267 Packet A inventory and ADR-0009 owner review." "No mass rename, no breaking cross-language API/schema change or automatic semantic-kernel adoption in this ticket."
- 2026-10-13 issue #317: `ui/src/fd/types.ts` has a narrower `Auth.type` union than `src/personal_world/worlds/models.py` (backend supports `cookie_session` and `password_grant`); record this as a confirmed enum/form mismatch, fix owned by #320.
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design."
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY survey of each API JSON shape across Python/Pydantic, OpenAPI, `ui/src/fd/types.ts`, `ui/src/generated/api-types.ts`, `ui/src/fd/api.ts` and mock fixtures; run `scripts/gen-openapi.py`/`scripts/contract-drift-check.py`/`ui/scripts/generate-api-types.mjs` WITHOUT changing generated artifacts; show one safe synthetic shape-change failure proves the guard catches mismatches. Write ONLY `docs/vnext/contracts-audit.md` (and commit it).
- out: any edit to generated files or `src/`/`ui/` code; no mass rename; no schema change.

## Acceptance
`test -s docs/vnext/contracts-audit.md && grep -q 'types.ts' docs/vnext/contracts-audit.md && grep -q 'contract-drift-check' docs/vnext/contracts-audit.md && grep -qi 'consumer' docs/vnext/contracts-audit.md`

## Evidence
`docs/vnext/contracts-audit.md` with a source-linked shape map, which files compile-import generated types, the drift-guard failure receipt, and a risk-ranked replacement order.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
