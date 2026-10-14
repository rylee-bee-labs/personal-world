# Packet: worlds-vnext / s0-retire

## Objective
Remove ONLY provably dead code from `personal-world` on ONE branch, one commit per retirement, each commit message carrying its consumer proof (grep + import evidence + tests) and its rollback (`git revert <sha>`); add `docs/vnext/retire.md` listing every retirement with its proof and rollback (#262).

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Work and commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design." then "Ok do add the api and visuals. Hah" — cleanup runs first; design and contract run beside it.
- 2026-10-13 issue #321: "Preserve legacy code until dependency/rollback evidence allows removal."
- 2026-10-13 issue #319: "name live consumers and preservation tests before changes."
- 2026-10-13 Claude (desk), answering the PR_LIMIT conflict: "s0-retire lands as ONE draft PR, one commit per retirement, each commit message carrying its consumer proof and rollback (git revert <sha>)."
- 2026-10-13 Claude (desk), same answer: protected code — auth, owner/CSRF, Memory store, art/canon, receipts, actions at-most-once — is Rylee's alone and needs a picker card first.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: `src/personal_world/**` and `ui/src/**` — remove ONLY a unit (module, file or export) with provable ZERO consumers: no importer anywhere under `src/`, `ui/`, `tests/`, `scripts/`, no route registration, no entry-point or dynamic-import reference, and no test that only exists to keep it. One commit per removed unit; its message names the grep/import evidence and the rollback command. Add `docs/vnext/retire.md` as the keep/retire ledger.
- out: any protected code (auth, owner/CSRF, Memory store, art/canon, receipts, at-most-once actions) — list such candidates under UNRESOLVED, do NOT remove them; any behavior change; any new feature; deleting data or branches; editing unrelated docs. If nothing is provably dead, return `partial` and say so plainly.

## Acceptance
`test -s docs/vnext/retire.md && grep -qi 'rollback' docs/vnext/retire.md && grep -qiE 'consumer|import' docs/vnext/retire.md`

## Evidence
`docs/vnext/retire.md` — every entry cites the grep/import evidence and a `git revert <sha>` rollback — plus the retirement commits themselves; any protected candidate is listed, not removed.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
