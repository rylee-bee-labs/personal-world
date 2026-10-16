# Stage C proof: the Connect contract, end to end, synthetic and read-only

> **Status: DRAFT proof, not deployed.** Synthetic data only, no network, no real credential, no write path.
> Scope came from the Stage C card. Rylee, 2026-10-14 23:30Z: "After the freeze".
> Project Home decision 145, 2026-10-13 16:09 CDT: "approve the boundaries; Sol #158 conditions become PRE-PROTOTYPE gates."
> Whether the run-2 day-alone test was scored before this started is **not something I could check from the clone**; the coordinator launched the task.

## What this proof is, and is not

The contract (`docs/vnext/connect-contract.md`) is a MOCKUP. This clone has no `/api/connect/v1` route, no `Auth.scope`
field and no `Action.state` field. So "the contract works end to end" is **only partly provable today**. The test file
`tests/vnext/test_connect_proof.py` proves the journey on the existing pieces (config store, runner, card service, confinement
seam, dispatcher) and pins each place the contract is ahead of the code as a **strict `xfail`** that names the missing piece. An
xfail is a gap, not a pass.

## How it was run (read this first)

The sandbox has **no `pytest` and no `fastapi`** (`python3 -m pytest` -> `No module named pytest`). Real pytest never ran.
I ran the file with a throwaway stand-in kept outside the repo (`/tmp/shim/run.py`, which supplies `pytest.raises`,
`importorskip`, `mark.xfail(strict=...)`, `tmp_path`, and a stub `fastapi` exposing only `HTTPException` and `Request` so
`authn.py` imports). That is a weaker check than pytest: it honours strict xfail and `raises=`, but it is my own runner. The
dispatcher tests ran under the stub; with real fastapi absent they would be SKIPPED (`importorskip`), meaning NOT RUN.

2026-10-16, after the second-model review (REQUEST-CHANGES) fixed the two condition-3 tests: re-run with a fresh stand-in
at the same distance from the repo (`/tmp/shim-run/run.py`, same surface). Counts unchanged: `{'passed': 10, 'failed': 0,
'skipped': 0, 'xfailed': 5, 'xpassed': 0}`. The out-of-scope xfail now fires as `AssertionError: not refused for scope`
(the request ran to the stubbed sender on an accepted address and was refused by nothing) instead of on a loopback
`confinement_denied`. Real pytest is still missing in the sandbox (no network); the host re-runs it.

## Commands and observed output

| # | Command | Exit | Observed tail |
|---|---|---|---|
| A | `test -s docs/vnext/proof/c-proof.md && python3 -m compileall -q src tests` | 0 | (no output) |
| B | `python3 -m pytest -q tests/vnext` | 1 | `/usr/bin/python3: No module named pytest` |
| C | `PYTHONPATH=src python3 /tmp/shim/run.py tests/vnext/test_connect_proof.py` | 0 | `{'passed': 10, 'failed': 0, 'skipped': 0, 'xfailed': 5, 'xpassed': 0}` |
| D | `python3 -c "import fastapi"` | 1 | `ModuleNotFoundError: No module named 'fastapi'` |
| E | `command -v node; ls ui/node_modules` | 1 | `ls: cannot access 'ui/node_modules': No such file or directory` |

Per-test lines from C:

```
PASS test_journey_empty_to_disconnected_and_still_usable
PASS test_c4_a_write_request_is_refused_before_the_sender
PASS test_c4_the_confinement_seam_refuses_a_mutating_read_effect
PASS test_c3_today_a_credential_applies_to_every_request_of_the_provider
PASS test_c3_an_unresolvable_reference_sends_nothing
XFAIL test_c3_auth_scope_exists_and_none_is_the_legacy_unscoped_state
XFAIL test_c3_unscoped_is_labelled_wherever_it_appears
XFAIL test_c3_out_of_scope_capability_fails_before_any_dns_lookup
PASS test_c1_project_home_without_a_verifier_is_not_approved_and_never_runs
PASS test_c1_a_project_home_refusal_leaves_the_action_unapproved
PASS test_c1_a_verifier_that_fails_hard_also_leaves_it_unapproved
PASS test_c2_an_unexposed_action_is_inert_for_an_agent_directly
PASS test_c2_an_unexposed_action_is_inert_through_other_paths
XFAIL test_c2_a_state_proposed_action_can_exist
XFAIL test_c2_a_state_proposed_action_is_inert_at_dispatcher_and_runtime
```

## The journey

`test_journey_empty_to_disconnected_and_still_usable` (command C, PASS): empty store -> provider with
`secret_ref: env:SYNTH_CONNECT_TOKEN` (the value is set only in the process env; the test asserts the YAML on disk holds the
reference and not the value) -> one card `healthy/current` -> timeout gives `unavailable/stale` with last-good values kept ->
unmapped status gives `unknown`, a missing field is `{"text": "unknown"}` (never 0) -> same read again recovers to
`healthy/current` -> `valid_link` keeps `/rooms/synth`, drops `https://elsewhere.example/` and `//elsewhere.example/` ->
delete of the provider is refused while referenced (`ConfigInvalid`), then board, card, request, provider are removed ->
store empty, `home_board_defs` is `None`, no further sender call, and a new connection renders again. The secret value appears in
no envelope; only the reference reached the sender.

Limits of that proof: the "retry" is the card service re-reading (`ttl_s: 0`), not `Dispatcher.request_retry`, which is the
write path and is out of scope. "Link out" is proven only for `room0.valid_link`, the one link rule in code; no card field
type renders a link. The contract's own `SourceState` has a `stale` value; the code expresses stale as `freshness: stale` with
`source_state: unavailable`. That is a naming difference to settle, not a failure.

## Condition 1: Project Home approval fails closed

Commands: C. Tests: `test_c1_project_home_without_a_verifier_is_not_approved_and_never_runs`,
`test_c1_a_project_home_refusal_leaves_the_action_unapproved`, `test_c1_a_verifier_that_fails_hard_also_leaves_it_unapproved`
(all PASS, exit 0, under the fastapi stub). Observed: with no verifier, a verifier returning False, and a verifier raising
`PermissionError("403 from Project Home")`, the authorization stays `pending`, `execute` raises `NotConsumable`, and the sender
is never called. The owner's own `approve` on a Project Home-governed provider raises `NotPermitted`.
**Not run:** the HTTP layer. `room_routes.py` maps `NotPermitted` to 403, but I did not exercise it because fastapi's
`TestClient` is unavailable here. The 403 is shown at the dispatcher, not on the wire. A real Project Home 403 is not simulated
end to end either; the verifier fixtures stand in for it.

## Condition 2: a proposed Action is inert

**Not proven as written.** `Action` has no `state` field (`src/personal_world/worlds/models.py:462`-`:476`, `extra="forbid"`),
so no Action can be `state=proposed`. The two strict xfails (`..._can_exist`, `..._is_inert_at_dispatcher_and_runtime`) fail with
`ValidationError` today. What I did prove is the nearest existing behaviour, which the contract (§6.1) cites as the reason a
proposed action is already inert: an `exposed: false` action. Direct: an agent's `request_authorization` raises `NotPermitted`
and `list_actions` is empty. Indirect: the agent's `execute` on the owner's pending authorization raises `NotPermitted` and
`request_retry` on an unknown execution raises `BadRequest`; the authorization stays `pending`, sender never called (command C,
PASS). That is a proxy, not the condition. I did not test the runtime (`Runner`) side of a proposed action: it has nothing to act on.

## Condition 3: scoped auth before the network, and "unscoped"

**Not met in code.** `grep -n scope src/personal_world/worlds/confinement.py` returns nothing: no scope check exists, and
`Auth` has no `scope` field. Observed with a counting resolver and the dial functions stubbed (no socket opened), on an
address confinement ACCEPTS (lan provider, synthetic private `10.9.9.9`): a credential scoped to capability `synth.health` <!-- pw-safety: synthetic -->
(carried by a synthetic provider in the contract's §6.1 shape, since `Auth` has no `scope` field) was asked to serve
`synth.other` — the request reached DNS (`lookups= 1`), reached the send seam (`dials= 1`, the scoped credential attached)
and was refused by nothing. So nothing refuses an out-of-scope capability before DNS. The strict xfail asserts the contract
order (§4.2: `auth_failed` before any DNS, no bytes); a scope check that lands only AFTER DNS keeps it red on the
`lookups == []` order assertion, and one that lands before DNS turns it into an XPASS(strict), which is loud. The word
"unscoped" occurs in no file under `src/` or `ui/src` (it appears only in the contract and a history doc), so a legacy
no-scope credential is not labelled anywhere. Three strict xfails record this. Passing, and no longer narrow:
`test_c3_today_a_credential_applies_to_every_request_of_the_provider` now proves BOTH requests of the provider reached the
sender and EACH carried `Authorization: Bearer <value>` (bite-checked 2026-10-16: an unavailable credential never reaches
the sender, an unattached one arrives bare, and the test fails on both breaks and recovers when restored), and
`test_c3_an_unresolvable_reference_sends_nothing` shows an unresolvable reference dials nothing.

## Condition 4: read-only end to end, keyboard, 390px

Read-only: PASS (command C). Every sender call across the journey was `effect="read"`, `GET`. A `POST` request behind a card is
refused as `confinement_denied` with zero sender calls, and the confinement seam refuses a `DELETE` under a read effect before
DNS. This covers the paths the proof reaches, not every route in the app.

**NOT RUN:** keyboard visibility of the stale, unknown and retry states, and the 390px phone layout. No node, no
`ui/node_modules`, no browser in this sandbox (command E). The repo has a `phone-390` Playwright project under `ui/e2e-fd`; I did
not run it and make no claim about it.

## RESULT / CHANGED / CHECKS / EVIDENCE / UNRESOLVED / NEXT

RESULT: partial. Condition 1 proven at the dispatcher only; condition 4 read-only proven, browser half NOT RUN; conditions 2 and 3 are not met because the code lacks `Action.state` and `Auth.scope`.

CHANGED: `tests/vnext/test_connect_proof.py` (new; the repo has no `tests/vnext`, and the existing `tests/worlds` layout needs relative imports from its own conftest, so this stands alone), `docs/vnext/proof/c-proof.md` (new). No protected code touched.

CHECKS: A `test -s ... && python3 -m compileall -q src tests` exit 0. B `python3 -m pytest -q tests/vnext` exit 1 (no pytest installed). C stand-in runner exit 0, 10 passed 5 xfailed 0 failed.

EVIDENCE: `{'passed': 10, 'failed': 0, 'skipped': 0, 'xfailed': 5, 'xpassed': 0}`; `No module named pytest`; `No module named 'fastapi'`.

UNRESOLVED:
- Real pytest and real fastapi never ran; command C used my own stand-in runner and a fastapi stub. Re-run `python3 -m pytest -q tests/vnext` in an environment with the `test` extra; the dispatcher tests skip (NOT RUN) without fastapi.
- Condition 2: `models.py:462` `Action` has no `state` (contract §6.1 proposes it). Gap in the contract's implementation, not a patched defect.
- Condition 3: `src/personal_world/worlds/confinement.py` has no scope check (`_prepare`, `:571`-`:600`, resolves DNS before anything credential-related) and `models.py` `Auth` (`:84`-`:100`) has no `scope`. Protected code; not patched.
- "unscoped" label exists nowhere in `src/` or `ui/src`.
- Condition 1 over HTTP (403 from `room_routes.py`) not exercised; Project Home 403 simulated by fixtures.
- Condition 4 keyboard and 390px: NOT RUN.
- Not checked: whether the run-2 day-alone test was scored before this started.

NEXT: Implement `Auth.scope` and `Action.state` as the contract's §6.1 proposes (owner decision on protected modules), flip the five xfails to plain tests, and run the `phone-390` e2e where a browser exists.
