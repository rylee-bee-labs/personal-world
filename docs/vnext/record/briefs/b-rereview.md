# Packet: worlds-vnext / b-rereview

## Objective
Fresh, independent re-review of the revised Connect contract: does `docs/vnext/connect-contract.md` (at the tip you are given) actually resolve the original review's 1 BLOCKER + 3 MAJOR findings, and what remains? Write ONLY `docs/vnext/connect-contract-rereview.md`. Different model family from the reviser (the revision was written by minimax).

## Where to work
Bazzite. `offload agent` worker in a throwaway clone of `personal-world` cut from the reviser's tip (the clone is based on `personal-world-gh`, which holds the revision). Work and commit ONLY in your clone; never touch the real checkout, never push.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 Sol #147 (advice): "design the interface independent of screens: discover -> secret reference -> safe test -> capability/meaning preview -> publish -> governed action link -> disconnect/recover. No second PH authority, no Companion history."
- 2026-10-13 brief.md Authority: "Rylee's alone: every merge, every deploy, every design pick, every product/trust boundary (identity, secrets, authority, Memory)". The contract may describe those boundaries; it must not change them.
- 2026-10-13 brief.md Routing: "No model grades only its own output: a reviewer or verifier is a different family from the author." You are a different family from the reviser (minimax) — say so in your return.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Scope
- in: write ONLY `docs/vnext/connect-contract-rereview.md` (and commit it). It must contain:
  1. A verdict table over the original's BLOCKER + 3 MAJORs — RESOLVED / PARTLY / NOT RESOLVED — each with the new contract § or `path:line` evidence you checked yourself. Do not take the revision's own claim as proof; open the files.
  2. Any NEW findings in the revision (each with § or `path:line`), tagged BLOCKER/MAJOR/MINOR/NIT.
  3. A final line: `FINDINGS: <n> (BLOCKER x, MAJOR y, MINOR z, NIT w)` for the benchmark.
- Original review (its file is not on this clone's base; this is the context): 14 findings — 1 BLOCKER (the contract requires persisted state with no declared home while §6/§7.1 assert "no new field/file/model/table"); 3 MAJOR: (a) §7.3 verifier-gap wording wrong — `dispatcher.py:277-278` raises `NotPermitted` and stores nothing; (b) §4 field ledger omits `CardDraft`, `Capability`, `ReadSpec`; (c) endpoint 12 does not exist; plus 4 MINOR, 2 NIT, 4 CONFIRMED. The revision added a `## Revision 1` section claiming to fix the BLOCKER and the three MAJORs.
- out: any other file; any code change; any push; any claim you did not verify against the files yourself.

## Acceptance
`test -s docs/vnext/connect-contract-rereview.md && grep -qiE 'resolved' docs/vnext/connect-contract-rereview.md`

## Evidence
`docs/vnext/connect-contract-rereview.md` — the verdict table with real `path:line` evidence, the new findings, and the `FINDINGS:` count line.

## Return (required, last block)
```
RESULT: done | partial | blocked
CHANGED: <files or commits, or none>
CHECKS: <command → observed result>, ...
EVIDENCE: <path:line or log path>
UNRESOLVED: <items, or none>
NEXT: <one concrete action for the coordinator>
```
