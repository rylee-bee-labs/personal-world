# d-lint — the ruff finding on the d-impl branch

**Task:** CI is red on `ruff` for this branch and the log is unreadable. Find the violation by reading the
code and fix it only if it is unambiguous.

**Tools:** `ruff` is **not installed** in this sandbox and there is no network, so **I did not run ruff**. The
findings below come from reading the code, from `grep`, and from a stdlib-`ast` scan I wrote to imitate the
configured rules (E, F, W, I, B with E501 ignored — read from `pyproject.toml`). I also **could not run pytest**:
`pytest` and `fastapi` are not installed in the sandbox. `python3 -m compileall -q src tests` passes.

## What I checked

Only the code the branch changed (`git diff <merge-base>..HEAD -- src tests`), cross-checked against a
whole-tree scan so that "green on main" files could serve as evidence for what this project's ruff tolerates.

- **F401 (unused import):** none. Every changed import line is used. The only unused-import-shaped hits in the
  whole tree are `from __future__ import annotations` (ruff ignores those) and re-exports in `chat.py` (unchanged
  by this branch). In particular `confinement.py` keeps `resolve_secret_ref`, `render_query` and the in-function
  `check_auth_header_name`, all used; moving `scope_allows` out left no import behind.
- **I001 (isort):** the changed import lines are sorted in the repo's established order (uppercase before
  lowercase, e.g. `Action, Auth, Board, BoardItem, Card, Provider, Request, scope_allows, send_scoped`), and the
  added per-function imports sit in already-sorted blocks (`db` before `dispatcher`). No reordering needed.
- **B008:** no new call-in-default. The ignored route modules are not otherwise touched in a way B008 applies to.
- **E7xx / W2-W3:** no trailing whitespace, no tabs, no >2-blank-line runs, no `== None/True/False`, no bare
  `except`. The one comparison-style line worth naming (`action.exposed is False`) is `is False`, which E712 does
  not touch.

## The violation (fixed)

**F841 — `tests/vnext/test_connect_proof.py:272`** (before the fix):

```python
        with _Dials() as dials:                      # dials is bound and never read in this function
```

Function: `test_c3_the_transport_enforces_the_verdict_it_is_given_and_does_not_decide_scope`. Its assertions
read `refused` and `lookups`; `dials` is never referenced anywhere in the function (the other two `_Dials()`
uses in the file, at lines 215 and 242, *do* read `dials.dialed` at lines 218 and 249).

**Fix applied:** `with _Dials() as dials:` → `with _Dials():`. `_Dials.__enter__` returning `self` is unused
here and `__exit__` still restores the stubbed dial functions, so nothing the test asserts changes. This is the
same shape the earlier branch commit `a100f34` ("lint(vnext): drop dead bindings in the Connect proof") used for
a dead `with ... as dials` binding.

**Why this is the one and not a tuple-unpack variable:** ruff's F841 fires on an unused `with ... as` target but
does **not** fire on unused names bound by tuple unpacking. That is not an assumption — files green on `main`
are full of them, e.g. `tests/worlds/test_dispatcher.py:93` `d, db, store, *_ = env` (with `db` unused) and
`tests/worlds/test_room0.py:153` `store, client, svc, clock, _ = env` (with `store`, `svc`, `clock` unused). A
whole-tree scan for unused `with`/`except` bindings finds exactly one, the `dials` above; the tuple-unpack name
it also reports in `dispatcher.py` (`action`) is exempt for the same reason.

**Introduced by:** commit `e5c9324` (the new transport-verdict test). It is one of the two code commits the brief
names.

## Candidates left alone (not certain)

- **`src/personal_world/worlds/models.py:518`** — a bare tuple expression statement:

  ```python
      if request.provenance == "owner" and not request.verified:
          request_d.pop("provenance"), request_d.pop("verified")
  ```

  This is the shape flake8-bugbear **B018** ("useless expression") hunts. I could not confirm that this
  project's ruff flags a tuple: it tolerates other bare expression statements that B018 sometimes covers —
  `tests/test_chat_tools.py:178` (`result.data["partial_findings"]`, a bare subscript) was added by the
  ruff-enabling commit `fa70dda` itself and kept green. Because I am not certain, I left the code alone rather
  than guess. If a second CI run still fails, this line is the next thing to look at; the safe rewrite (two
  statements, or `del request_d["provenance"]; del request_d["verified"]` semantics preserved) removes the
  question without changing behaviour.

## What I could not determine

- I did not run `ruff`, so I cannot show the CI log's exact rule code; F841 is inferred from the rule set, the
  code reading, and the repo's own tolerance pattern above.
- I did not run the project's tests: `pytest` is not installed (`python3 -m pytest` → "No module named pytest"),
  and `fastapi` is missing too, so `tests/vnext/test_connect_proof.py` cannot be collected here.

## Checks run

| command | result |
| --- | --- |
| `python3 -m compileall -q src tests` | exit 0 (acceptance) |
| `python3 -m pytest tests/vnext/test_connect_proof.py -q` | failed: `No module named pytest` (not installed) |
| `grep -n dials tests/vnext/test_connect_proof.py` | before fix: 272 bound, never read; after fix: only 215/218, 242/249 (bound and read) |
| stdlib-`ast` unused-import scan over `src` + `tests` | no F401 in changed files (only `__future__` / unchanged `chat.py`) |
| stdlib-`ast` unused `with`/`except` binding scan over `src` + `tests` | exactly one, now fixed |

## Return

```
RESULT: done
CHANGED: tests/vnext/test_connect_proof.py:272 (commit d51bd32), docs/vnext/d-lint.md
CHECKS: python3 -m compileall -q src tests → exit 0; python3 -m pytest tests/vnext/test_connect_proof.py -q → "No module named pytest" (not installed, not run); grep/AST scans → single unused with-binding, now gone
EVIDENCE: tests/vnext/test_connect_proof.py:272 (fixed); tests/worlds/test_dispatcher.py:93 and tests/worlds/test_room0.py:153 (green tuple-unpack evidence); src/personal_world/worlds/models.py:518 (B018 candidate, left)
UNRESOLVED: whether ruff also flags the bare tuple expression at src/personal_world/worlds/models.py:518 (B018); project tests could not be run (pytest/fastapi absent); ruff itself not run
NEXT: Re-run CI on this branch; if it is still red, treat models.py:518 as the B018 candidate and split the comma expression into two statements
```
