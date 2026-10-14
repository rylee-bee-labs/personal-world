# Packet: worlds-vnext / b-review

## Objective
Independently review `docs/vnext/connect-contract.md` (the Connect contract, worker commit b16ff82, 686 lines) against the decided #323 constraints: verify every keep/adapt/replace call and every field/endpoint claim against the actual code, and write the findings as `docs/vnext/connect-contract-review.md`. Advisory review only — change no other file. Count the findings (each with a severity) so the benchmark can read them. The author of the document is a different model family from you; your value is an independent check, including where the author may be wrong.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone whose base already contains the document under review; the rest of the tree is current `main`. Confirm it is there first: `test -s docs/vnext/connect-contract.md` — if that file is absent, stop and return `blocked` with the reason. Write and commit ONLY `docs/vnext/connect-contract-review.md` in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 issue #323: "Every field/endpoint must have a named consumer, a reason to exist and a security test." "Prefer normal providers exposing their own API, not bundling a new Sonarr/GitHub/Project Home implementation in Worlds."
- 2026-10-13 issue #321: "A new backend is allowed if justified by fit, not built merely because 'new' sounds better." "Credential *references* only, scoped access and no secret value round-trip into the browser."
- 2026-10-13 issue #319: "Worlds owns meaning. Providers own mechanics." External systems keep their own authority; Project Home owns its own consequential operations; Worlds shows/links, never mints competing authority.
- Status words stay honest: MOCKUP ≠ PROTOTYPE ≠ APPROVED; merged ≠ tested ≠ deployed.
- 2026-10-13 Claude (desk): protected code — auth, owner/CSRF, Memory store, art/canon, receipts, actions at-most-once — is Rylee's alone and needs a picker card first.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY check of `docs/vnext/connect-contract.md` against the real code. Verify at least: (a) the REPLACE verdict on `recipes.py` — the author claims `recipes.install()` has no caller; grep `src/` and confirm or refute; (b) the Project Home verifier gap — `dispatcher.py:160` accepts `project_home_verifier` that `production.py:95` never supplies; (c) the room0.py:493 selective-publish claim; (d) that every contract field/endpoint in the document names a consumer, a reason and a security test; (e) the threat and authority tables say nothing that contradicts #319/#321. Then write ONLY `docs/vnext/connect-contract-review.md` (and commit it) with: a verdict line, and a numbered list of findings, each tagged `BLOCKER | MAJOR | MINOR | NIT | CONFIRMED`, each citing `path:line` or a document section, plus a one-line count summary.
- out: any change to the contract itself or to any other file; any `src/`/`ui/`/tests edit; re-designing the contract; implementing anything.

## Acceptance
`test -s docs/vnext/connect-contract-review.md && grep -qiE 'BLOCKER|MAJOR|MINOR' docs/vnext/connect-contract-review.md && grep -qi 'consumer' docs/vnext/connect-contract-review.md`

## Evidence
`docs/vnext/connect-contract-review.md` — every finding cites a `path:line` in the code or a section of the contract; the count summary matches the findings listed.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
