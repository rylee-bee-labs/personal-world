# Packet: worlds-vnext / b-revise

## Objective
Revise `docs/vnext/connect-contract.md` so it honestly resolves every finding in `docs/vnext/connect-contract-review.md` (1 BLOCKER + 3 MAJOR + 4 MINOR + 2 NIT), keeping the provider-neutral design intact. One revision commit on the existing branch; do not open a PR.

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` cut from the existing branch `worlds-vnext-connect` (open draft PR #329, base main 786f506). Both `docs/vnext/connect-contract.md` and `docs/vnext/connect-contract-review.md` are on that branch. Work and commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 Sol #147 (advice): "design the interface independent of screens: discover -> secret reference -> safe test -> capability/meaning preview -> publish -> governed action link -> disconnect/recover. No second PH authority, no Companion history."
- 2026-10-13 brief.md Authority: "Rylee's alone: every merge, every deploy, every design pick, every product/trust boundary (identity, secrets, authority, Memory)". The contract may describe those boundaries; it must not change them.
- 2026-10-13 brief.md Acceptance: "The contract names a consumer, a reason and a security test for every field and endpoint."
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: `docs/vnext/connect-contract.md` only. Append a `## Revision 1` section: one row per review finding (id, severity, what changed and where, or a reasoned reject). Fix:
  - BLOCKER: sections 6 / 7.1 assert "no new field/file/model/table" while the contract also requires persisted state. Make the persisted-state story explicit and internally consistent (say exactly what persistence the contract needs and where it lives), or drop the persistence requirement with a stated reason.
  - MAJOR a: the verifier-gap wording is factually wrong. `src/personal_world/dispatcher.py:277-278` raises `NotPermitted` and stores nothing. Correct the claim and cite the real lines.
  - MAJOR b: the section 4 field ledger omits `CardDraft`, `Capability`, `ReadSpec`. Add each with its consumer, reason and a security test.
  - MAJOR c: endpoint 12 does not exist. Remove it or replace it with one that does (with consumer + security test).
  - MINOR/NIT: address where cheap and say so; otherwise reject with a reason.
- out: any other file; any new branch or PR; any change to protected code (auth, owner/CSRF, Memory store, art/canon, receipts, at-most-once actions); merges; deploys; secrets.

## Acceptance
`test -s docs/vnext/connect-contract.md && grep -qi 'Revision 1' docs/vnext/connect-contract.md`
(The recorded task acceptance is weaker; this is the bar you are judged on.)

## Evidence
The `## Revision 1` table in `docs/vnext/connect-contract.md` plus the diff against the branch tip. Every finding id from the review must appear, resolved or rejected with a reason.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
