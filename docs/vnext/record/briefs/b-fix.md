# Task b-fix — fix the public-safety CI failure in the Connect contract (#329)

Repo rylee-bee-labs/personal-world. You are on branch worlds-vnext-connect (draft PR #329). The revised contract and its independent re-review are already in your clone.

## What to do
CI fails on PR #329 at `tests/test_public_safety.py::test_tracked_text_files_carry_no_operator_topology`: a tracked doc under `docs/vnext/` contains private-IP / link-local / cloud-metadata literals. On the current tip the failure names `docs/vnext/connect-contract.md` lines 336 and 337 ("RFC1918", "CGNAT", "169.254.169.254"); the re-review doc may trip the same check.

First run the real test (it is pure-python and needs no network; you may import `tests/test_public_safety.py` with a tiny pytest stand-in) to see exactly which files it flags, then reword ONLY the flagged prose in those docs.

Reword ONLY the affected prose so it says the same thing without any private/link-local IPv4 literal, RFC number, or cloud-metadata address:
- "SSRF into loopback / RFC1918 / CGNAT, or DNS rebinding ..." -> e.g. "SSRF into loopback, private or link-local ranges, or DNS rebinding ..."
- "`network.lan: true` does not unblock 169.254.169.254" -> e.g. "`network.lan: true` does not unblock the cloud metadata address"

Keep every table row, path:line citation and security-test string otherwise intact. Add or remove nothing else.

Then verify: this test is pure-python and needs no network; run the repo's real `tests/test_public_safety.py` (you may import it with a tiny pytest stand-in) and confirm `test_tracked_text_files_carry_no_operator_topology` passes, or at minimum that `grep -nE '169\.254|RFC ?1918|CGNAT' docs/vnext/connect-contract.md` finds nothing.

Commit on the branch you were given. Do NOT push. Do NOT touch any other file.

## Decisions in force
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks"
- 2026-10-13 Rylee: "I want to use them. I'll get api credits every month and want them to be used to make everything better and more easy"
- 2026-10-13 brief: "Status words stay honest"; land draft only; never invent status.
- 2026-10-13 brief: "The contract names a consumer, a reason and a security test for every field and endpoint" — do not weaken any of these while rewording.
- 2026-10-13 brief: fix a technical failure at the root; zero errors, zero warnings.

## Return block
End with RESULT / CHANGED / CHECKS / EVIDENCE / UNRESOLVED / NEXT.
