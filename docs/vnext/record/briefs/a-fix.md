# Task a-fix — fix two ruff errors in the direction-1 mockup helper (#328)

Repo rylee-bee-labs/personal-world. You are on branch worlds-vnext-mockups (draft PR #328). Its tree is already in your clone.

## What to do
CI's three ruff jobs fail on PR #328 with exactly two errors in `docs/vnext/visuals/direction-1/check_floors.py`:
- line 184:30  **B905**  `zip()` without an explicit `strict=` parameter
- line 334:21  **W292**  No newline at end of file

Fix them: add `strict=False` to that one `zip()` call, and make the file end with a single trailing newline.

Touch no other file and no other content. If `ruff` is present, run `ruff check docs/vnext/visuals/direction-1/check_floors.py` and confirm 0 errors.

Commit on the branch you were given. Do NOT push.

## Decisions in force
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks"
- 2026-10-13 brief: land draft only; status words stay honest (MOCKUP != PROTOTYPE != APPROVED).
- 2026-10-13 brief: fix at the root; zero errors, zero warnings.

## Return block
End with RESULT / CHANGED / CHECKS / EVIDENCE / UNRESOLVED / NEXT.
