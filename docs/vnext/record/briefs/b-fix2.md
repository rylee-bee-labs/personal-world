# Task b-fix2 — reconcile the Connect contract's review findings and fix the worked sample (#329)

Repo rylee-bee-labs/personal-world. You are on branch `worlds-vnext-connect` at `80061dc` (draft PR #329). `docs/vnext/connect-contract.md` and its re-review `docs/vnext/connect-contract-rereview.md` are already in your clone. Read both before editing.

This closes Sol's relay #158 documentation items 4 and 5 so the Connect boundaries gate (b-gate) can be raised for Rylee.

## Item 4 — reconcile the re-review's findings in the contract

`docs/vnext/connect-contract-rereview.md` records new findings plus earlier MINOR/NIT. Fix the contract where the finding is correct; where a finding is already fixed or is a nit you choose not to take, say so in your return instead of silently dropping it.

1. **NEW-2 (MAJOR): "exactly four" is short by one.** §6.1 claims "exactly four additive optional model fields" (contract `docs/vnext/connect-contract.md:426`–`:428`), but §6.1:458 and §4.2:288 say `CredentialReference.rotated_at` is stored "in the provider file, if the owner sets it", while `src/personal_world/worlds/models.py:78`–`:101` has **no** `rotated_at` and sets `extra="forbid"` (`models.py:101`). Make the contract agree with itself, picking whichever the contract's own §4/§6 wording actually supports:
   - (a) `rotated_at` is **not** persisted by Worlds — reword :458 and :288 so the owner keeps the reminder outside Worlds and nothing is stored; or
   - (b) it **is** a fifth additive optional field — change the headline count.
   State which you chose and why in one line in your return, and make every mention agree.

2. **Regression: the `history` table.** §6's `worlds.db` row (contract `:643`) now says there is "no results or history table". The re-review shows `src/personal_world/worlds/memory_store.py:56` has `CREATE TABLE history (`. Correct the claim: name the real table (`history`, `memory_store.py:56`) and its actual columns, and keep the (still true) statement that `TestResult` is not persisted **and why**. Do not leave a false "no history table" claim.

3. **Other re-review MINOR/NIT and the earlier review's MINOR/NIT** where the contract is still wrong (a wrong `path:line` citation, a wrong field name, a claim with no named consumer or no security test). If the earlier review file `docs/vnext/connect-contract-review.md` is not in your clone, reconcile only the re-review and say so explicitly.

## Item 5 — fix the generic worked sample

In the worked integration sample (the generic HTTP example and the Project Home room/0 example), fix:
- `"credential": { "ref": "env:", "auth": "none", ... }` — an `env:` ref with an empty name is malformed and contradicts the threat table ("Worlds' own names are refused", contract `:545`). Use a concrete, obviously-synthetic name (e.g. `env:EXAMPLE_TOKEN`) or the correct no-credential shape for `auth: none`.
- `"publish_target": { "board": "home", "publish": ["Checks", "State"] }` — the publish list must name the fields the contract actually defines for the published result, not free labels; make the sample match the `TestResult`/publish schema the contract specifies, and make the shown `values` agree with it.
- Keep both examples internally consistent end to end (request → result → publish → render), with no real host, token, or private address.

## Constraints
- Edit ONLY `docs/vnext/connect-contract.md` (touch `docs/vnext/connect-contract-rereview.md` only if you must correct a factual citation there — say so if you do). Do not touch any other file.
- Do not weaken: every field and endpoint keeps a named consumer, a reason, and a security test.
- No real secrets, hosts, or private/link-local addresses; the repo's `tests/test_public_safety.py` must stay green.
- Commit on the branch you were given. Do NOT push.

## Verify before returning
- Re-read every section you changed and confirm the contract no longer contradicts itself on `rotated_at` or the `history` table.
- Run any cheap repo check available. `pytest`/a venv may be unavailable (no network) — if so, say so honestly rather than claiming it ran.
- Grep that no stale "no history table" or "exactly four" claim survives unless you deliberately justified it.

## Decisions in force
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks"
- 2026-10-13 brief (Authority): "Land as DRAFT only; never request auto-merge or mark ready; merges are Rylee's." A draft PR cannot merge — no `gh pr ready`, no `gh pr merge`.
- 2026-10-13 brief (Status words): "MOCKUP ≠ PROTOTYPE ≠ APPROVED; merged ≠ tested ≠ deployed." Do not claim a status you did not run.
- 2026-10-13 brief (Acceptance): "The contract names a consumer, a reason and a security test for every field and endpoint."
- 2026-10-13 brief: fix a technical failure at the root; zero errors, zero warnings.

## Return block
End with RESULT / CHANGED / CHECKS / EVIDENCE / UNRESOLVED / NEXT.
