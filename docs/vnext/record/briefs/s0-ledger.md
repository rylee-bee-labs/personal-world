# Packet: worlds-vnext / s0-ledger

## Objective
Write `docs/vnext/ledger.md`: a keep/retire/unknown ledger over current main, listing for each unit its exact live consumers, any preserved work and its rollback, the passing gates that prove it, and marking runtime behaviour `UNKNOWN` separately from source facts. Add a regression-check section (CI, zero-provider, Memory, at-most-once actions) (#312, #319).

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Work and commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design." then "Ok do add the api and visuals. Hah" — cleanup runs first; design and contract run beside it.
- 2026-10-13 issue #319: "Classify current implementation, with source-linked evidence" and "name live consumers and preservation tests before changes."
- 2026-10-13 issue #321: "Preserve legacy code until dependency/rollback evidence allows removal."
- 2026-10-13 Sol #147 (advice): "ledger lists exact current-main consumers, preserved work + rollback, passing gates, and marks runtime UNKNOWN separately from source facts; no casual deletion of art, auth, Memory, receipts or capability seams."
- 2026-10-13 Claude (desk): protected code — auth, owner/CSRF, Memory store, art/canon, receipts, actions at-most-once — is Rylee's alone and needs a picker card first.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY survey of `src/personal_world/**` and `ui/src/**`, then write ONLY `docs/vnext/ledger.md` (and commit it). For every module/export it must give a keep/retire/unknown call with `path:line` consumer evidence (grep/import), the preservation and `git revert <sha>`-style rollback for anything a later task removes, the gates that already pass, and an explicit `UNKNOWN` for anything only observable at runtime. Include a regression-check section naming the four invariants to re-check after any retirement: CI green, zero-provider boot, Memory store untouched, at-most-once actions.
- out: any edit to `src/`, `ui/`, `tests/`, config or existing docs; any deletion; any code change. List protected-code candidates, do NOT touch them.

## Acceptance
`test -s docs/vnext/ledger.md && grep -qiE 'keep|retire|unknown' docs/vnext/ledger.md && grep -qi 'rollback' docs/vnext/ledger.md && grep -qi 'consumer' docs/vnext/ledger.md`

## Evidence
`docs/vnext/ledger.md` — every row cites a `path:line` and its live consumer or says UNKNOWN; the regression section names the four invariants.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
