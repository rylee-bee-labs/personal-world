# Packet: worlds-vnext / b-contract

## Objective
Write `docs/vnext/connect-contract.md`: a typed, provider-neutral Connect contract with a threat/authority table, a version/migration impact table, a keep/adapt/replace call on `recipes.py` / `confinement.py` / `room0.py` / `config_store.py` with consumer evidence, and two worked integrations (generic HTTP and Project Home room/0) (#323).

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` based on the repo default branch (main). Commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 issue #323: "Every field/endpoint must have a named consumer, a reason to exist and a security test." "Prefer normal providers exposing their own API, not bundling a new Sonarr/GitHub/Project Home implementation in Worlds."
- 2026-10-13 issue #321: "A new backend is allowed if justified by fit, not built merely because 'new' sounds better." "Credential *references* only, scoped access and no secret value round-trip into the browser."
- 2026-10-13 issue #319: "Worlds owns meaning. Providers own mechanics." External systems keep their own authority; Project Home owns its own consequential operations; Worlds shows/links, never mints competing authority.
- Status words stay honest: MOCKUP ≠ PROTOTYPE ≠ APPROVED; merged ≠ tested ≠ deployed.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: READ-ONLY survey for consumer evidence, then write ONLY `docs/vnext/connect-contract.md` (and commit it). It must contain: the typed provider-neutral contract (discover/register/describe-capabilities, credential references, bounded test/transform to status/field/card with source+freshness, selective publish, explicit independently-governed action proposal + owner/Project Home approval link, disconnect/revoke); a threat/authority table; a version/migration impact table; a keep/adapt/replace call on `src/personal_world/worlds/recipes.py`, `confinement.py`, `room0.py`, `config_store.py` with `path:line` consumer evidence; and two worked read-only integrations (a generic HTTP service and Project Home room/0).
- out: any `src/`/`ui/` code change; any new backend; do not implement the contract; do not wire the old recipe UI (#320 parked).

## Acceptance
`test -s docs/vnext/connect-contract.md && grep -q 'room/0' docs/vnext/connect-contract.md && grep -qiE 'threat|authority' docs/vnext/connect-contract.md && grep -qiE 'recipes\.py|confinement\.py|room0\.py|config_store\.py' docs/vnext/connect-contract.md && grep -qi 'consumer' docs/vnext/connect-contract.md`

## Evidence
`docs/vnext/connect-contract.md`; each field/endpoint names its consumer, reason and security test; each keep/adapt/replace call cites consumer evidence.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
