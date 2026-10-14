# Packet: worlds-vnext / s0-preserved

## Objective
Decide and document whether `preserve/worlds-push-2026-10-13` (tip 37340cf) still carries anything `main` needs; write `docs/vnext/preserved-branch.md`. NEVER delete the branch.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 issue #321: "Preserve legacy code until dependency/rollback evidence allows removal."
- Mission rule: "Never delete the branch." `preserve/worlds-push-2026-10-13` must survive this mission untouched.
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design."
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Coordinator-provided comparison (already run on the real checkout — treat as given, verify the main side by reading files)
- `git log --oneline origin/main..37340cf`:
  - `37340cf fix(worlds): finish the front door's push, notify door and PWA`
  - `be324ac test(worlds): traversal tests assert nothing outside the static dir leaks`
  - `890edbf feat(worlds): the front door keeps Web Push, the notify door and the PWA`
- `git log --oneline 37340cf..origin/main`:
  - `786f506 chore(contracts): re-pin Play-Nice to 3282097 (#311)`
  - `77e15a7 Front door: Web Push, the notify door and the PWA (#310)`
- `git diff --stat origin/main 37340cf`: ONLY `.project/contracts/adoption.yaml | 2 +-` (one line).
- merge-base: `0feda5d`.

Read: main already received the front-door Web Push / notify door / PWA work as PR #310 plus a newer Play-Nice re-pin (#311); the only residual delta is one line in `.project/contracts/adoption.yaml`.

## Scope
- in: read-only inspection of `main`'s Worlds push/notify/PWA code and tests (e.g. `src/personal_world/push.py`, `push_routes.py`, the traversal tests) to confirm the work is present on main; then write ONLY `docs/vnext/preserved-branch.md` (and commit it).
- out: no merge, no rebase, no branch deletion, no `src/`/`ui/` edit.

## Acceptance
`test -s docs/vnext/preserved-branch.md && grep -qi '37340cf' docs/vnext/preserved-branch.md && grep -qi 'adoption.yaml' docs/vnext/preserved-branch.md && grep -qiE 'not needed|superseded|keep|do not delete' docs/vnext/preserved-branch.md`

## Evidence
`docs/vnext/preserved-branch.md` stating the decision (open a draft PR or not), the residual delta, and the explicit "do not delete the branch" note, backed by the receipts above plus file evidence from main.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
