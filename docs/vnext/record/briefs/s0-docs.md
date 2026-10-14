# Packet: worlds-vnext / s0-docs

## Objective
Fix the stale source-of-truth docs and add a canonical pointer table `docs/vnext/docs-truth.md` (#313). Docs only.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 issue #313: "This is confirmed documentation drift, not proof the current runtime is broken." Keep genuine durable contracts and historical content but "explicitly label superseded/scope-limited docs. Prefer generated route/status inventories when already available, not another hand-updated census."
- 2026-10-13 issue #313: "Never publish private locations, credentials or operator evidence in this public repository. Do not change deployment to make docs seem true."
- 2026-10-13 issue #321: front door is "Home · Connect · Memory · Settings", not an obsolete Station/init.
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design."
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: edit the drifted docs named by #313 — `AGENTS.md`, `.project/PLAN.md`, `ui/README.md`, `docs/INDEX.md`, `docs/QUICKSTART.md`, `SECURITY.md`; prefer pointing at generated inventories (`scripts/gen-openapi.py`, `scripts/contract-drift-check.py`) over hand copies. Write `docs/vnext/docs-truth.md` (the short canonical pointer table: route/build/install/test/backup commands, reproducible test receipts, broken-link fixes).
- out: any `src/` or `ui/` code change; any deployment/DNS/secret change; do not delete historical docs — label them.

## Acceptance
`test -s docs/vnext/docs-truth.md && grep -qiE 'QUICKSTART|AGENTS|INDEX' docs/vnext/docs-truth.md && grep -qiE 'canonical|source of truth' docs/vnext/docs-truth.md`

## Evidence
`docs/vnext/docs-truth.md` plus each edited doc; list every changed file in CHANGED with the one-line reason.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
