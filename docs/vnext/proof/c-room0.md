# c-room0 proof: Project Home as the contract's room/0, read-only and synthetic

> **Status: DRAFT proof, not deployed.** Synthetic data only: the room is a fixture, the address is the
> shipped `*.lan.example` placeholder (a reserved name that never resolves), the principal is the
> recipe's `your-person-id` placeholder. No network, no real credential, no write path, no owner ask
> during the test. Scope came from the c-room0 card; publishing to the shared gallery is a Claude-side
> action, not this worker's - nothing here is published.
>
> Reads as one suite with the Stage C proof: `docs/vnext/proof/c-proof.md` and
> `tests/vnext/test_connect_proof.py`. Same fixture style, same naming. This file adds the room/0 half.

## Decisions in force (quoted, not paraphrased)

- 2026-10-14 23:30Z Rylee, on the Stage C card: **"After the freeze"**. Scope: read-only end-to-end proof
  against the Connect contract (#329), synthetic data only, never deployed, DRAFT PR only. "No owner asks
  during the test." Her card also rules out auto-merge and any command that would mark a PR ready or merge
  it; this worker runs none of those.
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks".
- Sol #147 (advice, 2026-10-13): the read-only Project Home room/0 compatibility checkpoint must finish
  BEFORE any implementation architecture is committed, and it must verify zero-provider, stale/UNKNOWN
  and revoked-connection states.
- 2026-10-15 Claude, answering an [architecture] escalation on this task: "Do not amend the contract ...
  Keep the two proof tests xfail-strict, and put Auth.scope + Action.state ... into the d-plan as
  protected-code work ... the plan only describes it, nothing is built." Concretely for this worker: the
  contract §6.1 promises `Auth.scope` and `Action.state` and `confinement.py` resolves DNS before any
  scope check, so Sol's conditions 2 and 3 CANNOT pass against today's code. `tests/vnext/test_connect_proof.py`
  already pins both with `xfail(strict=True)`; those pins are left exactly as they are - not relaxed, not
  marked passed, and the contract is not edited to match the code.
- Protected invariants this proof binds: local Memory; accessibility floors; zero-provider usability;
  security/consent; no invented status; "external systems keep their own authority (Project Home owns
  operational authority, Companion owns chat, services own data)."

Nothing protected was modified. No protected module is edited anywhere in this change; the tests import
`room0`, `config_store`, `confinement`, `cards`, `runner` and `dispatcher` and call only public seams.

## What this proof is, and is not

The contract (`docs/vnext/connect-contract.md`) is a MOCKUP: no `/api/connect/v1` route exists, and
`Auth.scope` / `Action.state` are fields it *proposes* but the code does not have. So "Project Home is
the contract's room/0" is only **partly provable today**. This file proves the room/0 read surface as it
exists - `room0.py`, `config_store.py`, `confinement.py`, `dispatcher.py` - across the three states the
card names, and carries the Stage C conditions through: condition 1 fails closed and passes here;
conditions 2 and 3 are pinned strict `xfail` in the sibling file and are **not provable** against today's
code. An `xfail` is a gap, not a pass.

## How it was run (read this first)

The sandbox has **no `pytest`, no `fastapi` and no `node`** (commands B, D, F below). Real pytest never
ran. I ran the new file with a throwaway stand-in kept **outside the repo** (`/tmp/room0-shim/run.py`,
which supplies `pytest.raises`, `importorskip`, `mark.xfail(strict=...)`, `tmp_path` and a `fail`, over a
stub `fastapi` exposing only `HTTPException` and `Request` so `authn.py` imports). That is a weaker check
than pytest: it honours strict `xfail` and `raises=`, but it is my own runner. The three `test_c1_room0_*`
tests are dispatcher tests and call `pytest.importorskip("fastapi")` (as the Stage C file does): under
real pytest **without** fastapi they would be SKIPPED, i.e. NOT RUN; with the `test` extra they run for
real. A negative control confirmed the pin-assertion parser sees a relaxed pin as not-strict (strict
`False`), so the pins cannot be silently relaxed.

## Commands and observed output

| # | Command | Exit | Observed tail |
|---|---|---|---|
| A | `python3 -m compileall -q src tests` | 0 | (no output) |
| B | `python3 -m pytest -q tests/vnext` | 1 | `/usr/bin/python3: No module named pytest` |
| C | `PW_REPO=$PWD python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py` | 0 | `{'passed': 13, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}` |
| D | `python3 -c "import fastapi"` | 1 | `ModuleNotFoundError: No module named 'fastapi'` |
| E | `PW_REPO=$PWD python3 -I /tmp/room0-shim/run.py tests/vnext/test_connect_proof.py tests/vnext/test_room0_proof.py` | 0 | `{'passed': 23, 'failed': 0, 'skipped': 0, 'xfailed': 5, 'xpassed': 0}` |
| F | `command -v node; ls ui/node_modules` | 1 | `command -v node` prints nothing; `ls: cannot access 'ui/node_modules': No such file or directory` |
| G | `test -s docs/vnext/proof/c-room0.md && python3 -m compileall -q src tests` | 0 | (no output) |

Per-test lines from C (the whole new file):

```
PASS test_room0_zero_provider_is_empty_and_honest
PASS test_room0_a_card_for_an_absent_source_reads_not_configured
PASS test_room0_a_stale_card_is_never_current
PASS test_room0_last_good_after_a_failure_is_stale_not_live
PASS test_room0_unknown_is_unknown_never_healthy_or_zero
PASS test_room0_a_revoked_credential_is_named_and_not_retried
PASS test_room0_a_removed_provider_is_not_served_from_a_stale_snapshot
PASS test_room0_runs_synthetic_and_read_only_reaching_no_live_project_home
PASS test_c1_room0_a_project_home_answer_without_a_verifier_is_never_approved
PASS test_c1_room0_the_owner_cannot_approve_a_project_home_action_in_worlds
PASS test_c1_room0_a_verifier_that_does_not_confirm_leaves_it_unapproved
PASS test_c2_room0_the_state_proposed_pins_are_strict_and_the_gap_is_still_real
PASS test_c3_room0_the_scope_pins_are_strict_and_the_gap_is_still_real
```

Per-test lines from E (both files, so the inherited pins are visible as XFAIL, not quietly removed):

```
... (15 Stage C tests: 10 PASS, 5 XFAIL - see c-proof.md) ...
PASS test_room0_... (all 13 above)
{'passed': 23, 'failed': 0, 'skipped': 0, 'xfailed': 5, 'xpassed': 0}
```

## State 1 - zero-provider: nothing configured at all

Command (filtered to the two zero-provider tests; same stand-in runner as C):

```
PW_REPO=$PWD PW_K=room0_zero_provider python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_zero_provider_is_empty_and_honest
{'passed': 1, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)

PW_REPO=$PWD PW_K=absent_source python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_a_card_for_an_absent_source_reads_not_configured
{'passed': 1, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)
```

What that observed: with an empty `ConfigStore`, `RoomService.providers()` and `rooms()` and
`home_items()` and `needs()` are all `[]` (`room0.py:467-468`, `:493-500`, `:516-526`, `:528-531`),
`Room0Client.snapshot("project-home")` is `None` because `provider()` rejects a missing id before any
send (`room0.py:303-305`), `home_board_defs` is `None` (`cards.py:366-381`) and no sender call was made
(`fx.calls == []`). Nothing is invented: no "connected", no fabricated card, no error that reads like a
bug - the surface answers with the honest empty word. The one explicit word for "there is nothing here"
is a card whose source is absent reading `not_configured` with values `{"text": "unknown"}`, never `0`
(`cards.py:126-135`, `:173-178`; `cards.py:256-288`). **No browser check was run** (command F): the
empty-state *screen* is NOT RUN.

## State 2 - stale and UNKNOWN

Commands:

```
PW_REPO=$PWD PW_K=stale   python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_a_stale_card_is_never_current
PASS test_room0_last_good_after_a_failure_is_stale_not_live
PASS test_room0_a_removed_provider_is_not_served_from_a_stale_snapshot        (state 3b, matched on "stale")
{'passed': 3, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)

PW_REPO=$PWD PW_K=unknown python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_unknown_is_unknown_never_healthy_or_zero
{'passed': 1, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)
```

**Stale.** A room card whose own `freshness.observed_at` is older than its `stale_after_s` renders
`freshness: "stale"` and never `"current"` (`room0.py:433-434`). An unreachable room keeps its
last-good cards and marks them stale (`room0.py:406-409`), so the envelope reads
`source_state: "unavailable"`, `freshness: "stale"` with `evidence.error_class: "connection"`
(`room0.py:420-447`). Observed: `env["freshness"] == "stale"` in both cases, and the first case keeps
the fact readable (`body` text present) while the *lapsed freshness* is what is flagged. A claim I do
**not** make: the lapsed card's `source_state` is still `healthy` - the room's own status - because
availability and freshness are separate fields (contract §4.3; `room0.py:420-421`). The staleness is
visible in `freshness`; the card is never labelled `current`.

**UNKNOWN.** A room status word room0 does not recognise is carried as `unknown`, never mapped to
`healthy` (`room0.py:390`, `STATE_FOR_STATUS` at `:36`), and the status card's value is the literal text
`"unknown"` (`room0.py:426`). A card whose title/body/freshness cannot be read honestly becomes
`source_state: "degraded"` with every value `{"text": "unknown"}` and no `raw` key - never a fabricated
`0` (`room0.py:428-429`). An unrecognised tone is shown as the neutral `update`, logged
(`room0.py:155-158`). Observed: `status_env["source_state"] == "unknown"` and
`broken["values"]["body"] == {"text": "unknown"}` (no `raw`). **No browser check** - keyboard visibility
of stale/unknown is NOT RUN (command F).

## State 3 - revoked connection

Commands:

```
PW_REPO=$PWD PW_K=revoked          python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_a_revoked_credential_is_named_and_not_retried                 (exit 0)
PW_REPO=$PWD PW_K=removed_provider python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_a_removed_provider_is_not_served_from_a_stale_snapshot        (exit 0)
```

**The revocation that the code can name: the room answers 401/403.** `room0.py:364-365` classifies
401/403 as `auth_failed`; `STATE_FOR_ERROR` maps that to `needs_attention` (`room0.py:37-39`), and
`rooms()` reports `reachable: False` with `error_class: "auth_failed"` (`room0.py:528-531`). Observed:
after a good snapshot, a 403 makes `snapshot.error_class == "auth_failed"` while the last-known cards
are copied forward and marked stale (`room0.py:406-410`), and the card envelope reads
`freshness: "stale"`, `source_state: "needs_attention"`, `evidence.error_class: "auth_failed"` - the
revocation is named and nothing stale is shown as live.

**No silent retry.** A failing room is backed off 30 s doubling to 5 min (`FAIL_BACKOFF_S` at
`room0.py:285`; the "left alone until its back-off ends" branch at `room0.py:311-312`). Observed: a
second `snapshot()` inside the window returned the same failing snapshot with `len(fx.calls)` unchanged -
there is no second send.

**Worlds drops its own reference.** With the provider config removed, `snapshot()` returns `None`
before any send (`room0.py:303-305`), `RoomService.card(...)` returns `None`, and `rooms()` /
`home_items()` are empty; the cached last-good snapshot stays in the client's memory but is never served.
Observed: `client.snapshot == None`, `card == None`, `fx.calls` unchanged after removal. Note
`config_store.py:358-369` refuses a delete while something still references the object - so in
production the request/action files an adopted room action created must be removed first; the read-only
proof removes a bare provider.

**Honest gap.** No room0 surface *names* a revocation as such: there is no room0 revoke/disconnect route
and no `revoked` state in `room0.py`. The contract's endpoints 13-14 and `DisconnectResult` /
`RevocationResult` (contract §3, §4.6) are MOCKUP, and `confinement.py:298` (`_session_forget`) is the
only credential-purge in code - it drops a *held* cookie/token, not a room0 bearer reference. So "names
the revocation" is proven only for the room-says-403 path (`auth_failed`); removing the config makes the
room vanish rather than say "revoked". That is recorded under UNRESOLVED, not papered over.

## Inherited Stage C condition 1 (provable - passes): a Project Home-governed answer fails closed

Command:

```
PW_REPO=$PWD PW_K=c1_room0 python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_c1_room0_a_project_home_answer_without_a_verifier_is_never_approved
PASS test_c1_room0_the_owner_cannot_approve_a_project_home_action_in_worlds
PASS test_c1_room0_a_verifier_that_does_not_confirm_leaves_it_unapproved
{'passed': 3, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)
```

Observed: a room0 provider defaults to Project Home governance (`models.py:203-204`,
`Provider.governed_by_project_home`), and `RoomService.answer` routes a supplied
`project_home_approval_id` into `dispatcher.record_project_home_approval` (`room0.py:598-599`,
`room_routes.py:72`). With no verifier wired - production supplies none, `production.py:95` -
`record_project_home_approval` raises `NotPermitted` on its first statement, **before the write
transaction** (`dispatcher.py:277-278`). Observed in all three tests: `NotPermitted` propagates, the
authorization row stays `pending` with `authority IS NULL`, `executions` is empty, and the write sender
(`writes == []`) is never called - nothing reaches the room. The room action the owner adopts is
`exposed: false`, `approval: "always"`, `idempotency: "required"` (`room0.py:627-628`), so it is never an
agent tool and never a no-approval write. Without a `project_home_approval_id`, the owner's own
`dispatcher.approve` refuses a Project Home-governed operation (`dispatcher.py:268-269`).

**Not run:** the HTTP 403 on the wire. `room_routes.py:75-76` maps `NotPermitted` to `HTTPException(403)`,
but fastapi's `TestClient` is unavailable here, so the 403 is shown at the dispatcher, not over HTTP. A
real Project Home 403 is simulated only by fixtures (the refusing-verifier test); end to end over HTTP
is NOT RUN.

## Inherited Stage C conditions 2 and 3: unmettable today, pins asserted

Command:

```
PW_REPO=$PWD PW_K=pins_are_strict python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_c2_room0_the_state_proposed_pins_are_strict_and_the_gap_is_still_real
PASS test_c3_room0_the_scope_pins_are_strict_and_the_gap_is_still_real
{'passed': 2, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)
```

These two tests do not make the missing features pass. They assert the **pins** and the **gap**, so a
future fix turns the pins green loudly (strict `xfail` -> XPASS -> failure) and this file fails too until
it is updated. The pins live in `tests/vnext/test_connect_proof.py` and were **not edited**:

- `test_c2_a_state_proposed_action_can_exist` and
  `test_c2_a_state_proposed_action_is_inert_at_dispatcher_and_runtime` - both
  `@pytest.mark.xfail(strict=True, raises=ValidationError, ...)`.
- `test_c3_auth_scope_exists_and_none_is_the_legacy_unscoped_state` (`strict=True, raises=ValidationError`),
  `test_c3_unscoped_is_labelled_wherever_it_appears` (`strict=True`),
  `test_c3_out_of_scope_capability_fails_before_any_dns_lookup` (`strict=True`).

**Condition 2 (a proposed Action is inert) - NOT MET.** Contract §6.1 proposes `Action.state`
(`connect-contract.md:452`), whose default would make every existing action file behave as today. Today
`Action` has no `state` field (`models.py:460-476`) and is `extra="forbid"` (`models.py:39`), so any
`Action(state="proposed")` raises `ValidationError` - which is exactly what the two strict pins record.
Observed: `"state" not in Action.model_fields`.

**Condition 3 (scoped auth before the network) - NOT MET.** Contract §4.2 and §6.1 promise
`CredentialReference.scope` checked "before any socket opens" (`connect-contract.md:287`) and
`Auth.scope` (`connect-contract.md:449`). Today `Auth` has no `scope` field and is `extra="forbid"`
(`models.py:78-159`, `extra` at `:101`), and `confinement.py` contains no scope check: `_prepare`
(`confinement.py:571`) resolves DNS at `:593` and the credential is only applied afterwards, in
`_send`'s `_auth_headers` call at `:565`. Observed: `"scope" not in Auth.model_fields` and the string
`scope` appears nowhere in `confinement.py`'s source. So an out-of-scope capability is not refused before
DNS; the three strict pins record it. Per the 2026-10-15 decision this is protected-code work for the
d-plan, described only - nothing was built.

## Read-only and synthetic-only

Command:

```
PW_REPO=$PWD PW_K=no_live_project_home python3 -I /tmp/room0-shim/run.py tests/vnext/test_room0_proof.py
PASS test_room0_runs_synthetic_and_read_only_reaching_no_live_project_home
{'passed': 1, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0}          (exit 0)
```

Every room call across the proof was a fixture call with `effect == "read"` and `method == "GET"`; the
test runs the home/rooms/needs/candidates paths under a guard that replaces `confinement._exchange`,
`confinement._session_send` and `confinement._system_resolver` with a failing stub, so reaching the live
seam would fail the test. `base_url` is `https://project-home.lan.example` - the shipped placeholder
(`config/recipes/project-home/provider.yaml`), a reserved `*.example` name that never resolves - and
`principal_id` is the recipe's `your-person-id`. This covers the paths the proof reaches, not every route
in the app, and it does not claim no other code could dial.

## NOT RUN

- Real `pytest` (command B: `No module named pytest`).
- Real `fastapi`: command D; the three `test_c1_room0_*` tests ran under the shim's fastapi stub and are
  SKIPPED under real pytest without the `test` extra.
- The HTTP 403 on the wire (`room_routes.py:75-76`) - no `TestClient`.
- Keyboard visibility of the stale/unknown/retry states and the 390px phone layout - no node, no
  `ui/node_modules` (command F). The repo has `ui/e2e-fd` Playwright specs; I did not run them and make
  no claim about them. Human screen-reader and physical-device checks are NOT RUN until a human runs them.
- Whether the run-2 day-alone test was scored before this started - not checkable from the clone.

## UNRESOLVED

- Real pytest and real fastapi never ran; command C/E used my own stand-in runner and a fastapi stub.
  Re-run `python3 -m pytest -q tests/vnext` in an environment with the `test` extra.
- Condition 2: `models.py:460-476` `Action` has no `state` (contract §6.1 proposes it). Protected code;
  not patched.
- Condition 3: `confinement.py:571-613` resolves DNS before any credential/scope decision; `models.py:78-159`
  `Auth` has no `scope`. Protected code; not patched.
- No room0 "revoked" state or route: removing the provider makes the room vanish rather than name a
  revocation; the contract's endpoints 13-14 / `RevocationResult` are MOCKUP. Only the room-says-403
  path names it (`auth_failed`).
- Condition 1 over HTTP (403 from `room_routes.py`) not exercised; Project Home 403 simulated by fixtures.
- Browser/keyboard/390px: NOT RUN (above).

## Return

```
RESULT: done (read-only, synthetic). States 1, 2 and 3 proven by command and observed output; inherited
condition 1 passes; conditions 2 and 3 are unmettable today and stay pinned strict xfail, asserted here.

CHANGED: tests/vnext/test_room0_proof.py (new; 13 tests, reuses the Stage C fixture style),
docs/vnext/proof/c-room0.md (new). No protected code touched. The stand-in runner lives outside the
repo at /tmp/room0-shim/run.py and is not part of the change.

CHECKS: A python3 -m compileall -q src tests -> exit 0. B python3 -m pytest -q tests/vnext -> exit 1
(No module named pytest). C stand-in runner on the new file -> exit 0, 13 passed 0 failed. E stand-in
runner on both vnext files -> exit 0, 23 passed 0 failed 5 xfailed 0 xpassed. Per-state targeted runs
(zero_provider, absent_source, stale, unknown, revoked, removed_provider, c1_room0, pins_are_strict,
no_live_project_home) -> all exit 0.

EVIDENCE: observed summaries {'passed': 13, 'failed': 0, 'skipped': 0, 'xfailed': 0, 'xpassed': 0} and
{'passed': 23, 'failed': 0, 'skipped': 0, 'xfailed': 5, 'xpassed': 0}; No module named pytest;
No module named 'fastapi'. Pins at tests/vnext/test_connect_proof.py:220-226, 341-350; gaps at
src/personal_world/worlds/models.py:460-476 and confinement.py:571-593.

UNRESOLVED: (1) real pytest/fastapi never ran (stand-in runner + fastapi stub); (2) condition 2 -
Action has no state (models.py:460-476); (3) condition 3 - no Auth.scope (models.py:78-159) and DNS
resolves before any scope check (confinement.py:593); (4) no room0 revoke route/state - only the
room-says-403 path names a revocation; (5) condition 1 over HTTP not exercised; (6) NOT RUN: keyboard,
390px, real browser, real fastapi.

NEXT: Implement Auth.scope (checked before DNS) and Action.state as contract §6.1 proposes, as
protected-code work for Rylee's d-plan; then flip the five strict xfail pins and run the phone-390 e2e
where a browser exists.
```
