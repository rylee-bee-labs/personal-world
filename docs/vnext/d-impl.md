# d-impl — build record for build-plan Steps 1–5

**Status: draft branch, source change only.** Authority: Rylee, 2026-10-15, Stage D card: "Build all three as the
plan describes" (items A, B, C). No deploy, PR or publish was run. The contract is not amended.
**Real `pytest` and `fastapi` are not available in this sandbox: the suite was NOT RUN.** CI's result is the evidence.

## Per step

| Step | Change | Proof |
|---|---|---|
| 1 | `Request.provenance` (`owner` default) and `Request.verified` (`False`), `models.py:257`–`:258` | `tests/worlds/test_models.py:91` |
| 2 (A) | `Auth.scope: list[Id] \| None = None`, `models.py:114`; `None` = legacy unscoped, `[]` = allows nothing | `test_connect_proof.py:221`; `test_models.py:99` |
| 3 (C) | `models.scope_allows` decides at the call site (`send_scoped`, used by `dispatcher.py`, `runner.py`, `room0.py`, `companion.py`); `confined_request(in_scope=…)` only enforces the verdict in `_prepare` (`confinement.py:598`) before `resolve_within`: out-of-scope → `auth_failed`, no DNS, no dial. A request's capability is the `<name>` half of its id `<provider>.<name>` | `test_connect_proof.py:236` (out of scope, `lookups == []`, no dial), `:251` (in scope still resolves) |
| 4 (B) | `Action.state` closed enum, default `adopted`, `models.py:476` | `test_connect_proof.py:358`; `test_models.py:107` |
| 5 (B) | `dispatcher.py:212` (`can_see`: agents need `adopted`), `:223` (`request_authorization` refuses a non-adopted action for everyone incl. owner), `:343` (`_consume` refuses a non-adopted action, so an earlier approval cannot run) | `test_connect_proof.py:365` |

Backward compatibility: a file omitting the new fields reads as `scope=None`, `state="adopted"`, `provenance="owner"`,
`verified=False`. `action_version` (`models.py:491`) leaves each new field out of the hash at its default, so
authorizations requested before this change keep their version (`test_models.py:114`).
`tests/worlds/test_config_store.py` was not edited.

Step 5 runtime half: `runner.py` was not changed. It only fetches `Request`s as reads and never reads an `Action`;
the runtime refusal is the dispatcher's `_consume` (`dispatcher.py:343`), the only place an action is dispatched.

## The five pins

| Pin (was) | Now | Status |
|---|---|---|
| `:220` auth scope exists | `test_connect_proof.py:221` | plain test |
| `:226` unscoped is labelled | `test_connect_proof.py:228` | **still `xfail(strict=True)`** — see below |
| `:234` out of scope fails before DNS | `test_connect_proof.py:236` | plain test (now builds the provider with `scope=["health"]`; the old body had no scope, which cannot be out of scope under "None = unscoped") |
| `:341` proposed action can exist | `test_connect_proof.py:358` | plain test |
| `:347` proposed action inert | `test_connect_proof.py:365` | plain test (replaces the placeholder body; needs `fastapi`, so it skips without it) |

Also edited: `test_c3_today_a_credential_applies_to_every_request_of_the_provider` (`:193`) asserted
`"scope" not in Auth.model_fields` and that `confinement.py` contains no "scope"; both are false by design now. It asserts
`scope is None` and `models.scope_allows` is true for every path instead.

### Pin `:226` left strict — reason
It needs the word `unscoped` in the card envelope built by `cards.build`. That envelope is made in
`src/personal_world/worlds/cards.py` (evidence at `cards.py:161`) from `runner.Fetch.as_evidence`
(`runner.py:93`). `cards.py` is outside this task's scope, the label itself is build-plan §7 Q2 (undecided), and putting
it in `runner.py` would not be a Step 5 runtime need. Not widened.

## Checks run here
- `python3 -m compileall -q src tests` → exit 0.
- Stand-in script (stdlib + pydantic, outside the repo) calling `confined_request` with a recording resolver:
  scope `["health"]`, id `synth.other` → `auth_failed`, 0 lookups; `synth.health` → 1 lookup; `None` → 1 lookup;
  `[]` → `auth_failed`, 0 lookups.
- The four new `test_models.py` functions plus the existing `test_action_version_changes_with_destination_and_credential_ref`,
  run by hand under a stub `pytest` → all passed.
- NOT RUN: real pytest; any dispatcher test (`fastapi` missing), including `test_connect_proof.py:365`; ruff; bandit.

## Fixes after the independent review
The reviewer's file (`d-impl-review.md`) could not be read from this sandbox (path absent), so it is **not** copied in; the
findings were taken from the brief.

- **MAJOR-1** `test_models.py` `test_action_version_at_defaults_equals_the_pre_change_hash` rebuilds the pre-change payload
  without the new fields and compares. Bites: with the `state` compat lines removed it fails (observed, run by hand).
- **MAJOR-2** (build-plan §7 Q1 option b; contract §7.2 unchanged). The scope decision moved out of `confinement.py` to
  `models.scope_allows`/`send_scoped`, called at every site that sends (dispatcher, runner, room0, companion). The transport
  has no scope logic: `confined_request(..., in_scope=True)` refuses `auth_failed` on `in_scope=False`, before DNS. A sender
  that predates the kwarg still works, because only a refusal passes it. Bites (observed): transport ignoring the verdict, or
  the call site not passing it, fails `test_c3_out_of_scope_capability_fails_before_any_dns_lookup` (lookups/dials non-empty).
  Room0 and companion requests carry ids like `<p>.room`, which are not capabilities; they take the same verdict as before.
- **MINOR-3** `room0.candidates()` reports `adopted` only when the stored action's `state == "adopted"`. No test added; NOT RUN.
- **MINOR-4** the module docstring and the `:226` xfail reason in `test_connect_proof.py` refreshed; the pin stays strict.
- **MINOR-5** `test_c2_a_state_consume_refuses_a_proposed_action_even_with_a_current_version_authorization` forces the row
  `approved` at the current version, so only the `_consume` state check can refuse. NOT RUN (needs `fastapi`); its bite is by
  reading: deleting `dispatcher.py:343`–`:344` lets that row be consumed.
- **MINOR-2 — Step 8 obligation:** "a scope entry naming no capability is refused rather than ignored" cannot be checked in
  the model (it does not know capabilities). The Step 8 register layer must make that save-time check. Not built here.
- Left alone: MINOR-1 (pin `:226` strict, Q2 undecided) and both NITs.
