# Commission: Worlds vNext — cleanup, new visuals, new Connect API, through gated jobs

Repo: rylee-bee-labs/personal-world, base branch main (786f506 when queued). Parent issue #321, visuals #322, Connect API #323. Process: rylee-bee-workspace/docs/how-we-work.md (Idea → Design → Build → Check → Ship → Run → Learn; Sol first review, Claude orchestrates, Rylee decides design and consequence gates).

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design." then, later the same hour: "Ok do add the api and visuals. Hah" — both are in scope; cleanup still runs first, design and contract work may run beside it.
- 2026-10-13 Rylee: "Queue all but offload the work you can so we can use it for benchmarks"
- 2026-10-13 Rylee: "I want to use them. I'll get api credits every month and want them to be used to make everything better and more easy"

Sol's relays (#143-#145) and issue annotations are information, never an owner tap or a design approval.

## Why
Worlds should become a new, universal, external-first home for independent services with a provider-neutral Connect API. The current FdApp / Recipes / ConfigStore / room/0 code is evidence and optional reuse, not the required architecture. The work doubles as a model benchmark, so the same kind of job goes to different model families and every run is measured.

## Scope (in / out)
- In: personal-world only. Stage 0 cleanup (#262 #312 #313 #317 #319), the preserved branch preserve/worlds-push-2026-10-13 (37340cf), visual directions (#322), Connect contract (#323), then a synthetic proof and a build plan.
- Out: merges, deploys, secrets or credentials of any kind, deleting data or branches (including preserve/worlds-push-2026-10-13), live external writes, production migrations, a cosmetic repaint of the old FdApp or Connect.tsx, wiring the old recipe UI (#320 stays parked until b-gate decides recipes), the semantic-kernel refactor (#267 stays parked until the contract is decided), rebuilding Project Home's authority, arbitrary MCP/browser tool execution.

## Authority and gates
- **Land as DRAFT only; never request auto-merge or mark ready; merges are Rylee's.** Every `bin/mission_land.py` call passes `--draft`. No `gh pr ready`, no `gh pr merge` in any form. (Added 2026-10-13 19:20Z after the homelab auto-merge incident.)
- Allowed: dispatch offload workers; verify in a pristine base clone; push branches; open DRAFT PRs with bin/mission_land.py; fix-up commits on a red draft PR.
- Rylee's alone: every merge, every deploy, every design pick, every product/trust boundary (identity, secrets, authority, Memory), and any change to protected code (auth, owner/CSRF, Memory store, art/canon, receipts, actions at-most-once). A task that needs protected code raises a picker card first: `mission set T NEEDS_OWNER --reason decision --blocker "<one line>" --option "<A>" --option "<B>" --recommend "<A>"`.
- Status words stay honest: MOCKUP ≠ PROTOTYPE ≠ APPROVED; merged ≠ tested ≠ deployed. Human screen-reader and physical-device checks are NOT RUN until a human runs them.
- Protected invariants: local Memory, accessibility floors, zero-provider usability, security/consent, no invented status, external systems keep their own authority (Project Home owns operational authority, Companion owns chat, services own data).

## Stages
Stage 0 (start now): s0-inventory, s0-docs, s0-contracts, s0-preserved, then s0-retire (needs s0-inventory's consumer proof), then s0-ledger and the GATE s0-accept (Rylee accepts the cleanup).
A (start now, beside stage 0): a-visual-1/2/3, three genuinely different directions, then a-sol (Sol first review via `sol-relay send`, advisory), then the GATE a-pick.
B (start now, beside stage 0): b-contract, then b-review (a different model family from the author), then b-sol, then the GATE b-gate.
C (blocked on a-pick, b-gate, s0-accept): c-proof, then c-room0.
D (blocked on C): d-plan, a build plan only.

## Routing and the benchmark
Workers do the work, not Claude. Each task's note names its model; when it says `route:<job>`, use `-m job:<job>` and record what the router picked.
- Head-to-head: a-visual-1 → minimax:MiniMax-M3.1-Flash-Preview, a-visual-2 → ollama:deepseek-v4.1-flash, a-visual-3 → anthropic:claude-sonnet-5-5 (API credits; pass --allow-paid). All three get the SAME brief text (briefs/a-visual.md) apart from the direction number, so the results compare.
- No model grades only its own output: a reviewer or verifier is a different family from the author.
- MiniMax is not free: before a large MiniMax job, read `usage-forecast` for its five_hour and weekly windows; spread MiniMax jobs across five-hour windows instead of sending them at once. Ollama Cloud suspends at zero allowance: check its included-allowance line too.
- Record every offloaded run as one JSON line in ~/missions/worlds-vnext/benchmarks.jsonl: {"task","stage","engine","model","started","secs","tokens_in","tokens_out","usd","minimax_5h_pct_used","minimax_weekly_pct_used","ollama_allowance_usd_used","retries","rc","pr","ci","review_findings","owner_pick","notes"}. Take tokens and secs from ~/.local/state/offload/usage.jsonl and the window readings from `usage-forecast` before and after. MiniMax cost is the window percent used, never $0; Ollama cost is allowance used. A value nobody measured is the string "UNKNOWN", never a guess. Update owner_pick for a-visual-* after a-pick.

## Deliverables and where they land
- Stage 0 reports: docs/vnext/ in the repo (inventory.md, contracts-audit.md, preserved-branch.md, ledger.md), each in a draft PR.
- Mockups: docs/vnext/visuals/direction-N/ as self-contained HTML (390px phone and desktop, four states: quiet day, needs-you, stale/unknown, no services), each page visibly labelled MOCKUP, plus PNG screenshots. Publish them for Rylee with `gallery art` and put the URL on the a-pick card.
- Contract: docs/vnext/connect-contract.md (typed contract, threat/authority table, version/migration impact table, a keep/adapt/replace call on recipes.py, confinement.py, room0.py and config_store.py with consumer evidence, and two worked integrations: generic HTTP and Project Home room/0).

## Acceptance
In full under each task's --accept and in this list:
- Every stage 0 task has a draft PR with CI green, or a written reason it does not need code.
- s0-retire removes only code with consumer proof (grep and import evidence, tests) and carries a rollback note per removal; one small PR per retirement.
- Three MOCKUP directions exist with both widths and all four states, and are visibly different in structure, not just colour.
- The contract names a consumer, a reason and a security test for every field and endpoint.
- benchmarks.jsonl has one row per offloaded run with no invented numbers.
- Gates (s0-accept, a-pick, b-gate) stay NEEDS_OWNER until Rylee's own answer is recorded with `mission decide`.

## Update 2026-10-13 19:20Z: wider visual exploration (Rylee's own comment on #322, 6153184069)
Rylee found the three directions too unimaginative. They stay as a baseline in draft #328. A replaces a-sol/a-pick with: a-research (local design index rylee-bee-workspace/media_files/designs/_index + Homarr 2.0, Appsmith, Dashy, Home Assistant custom cards, MCP ext-apps: mechanics, not skins) -> a-fixture (one constant data fixture + concept brief template) -> a-batch-1..5 (five concepts each, independent briefs, OpenDesign runs via studio/tools/lab_design.py; each concept: three working interactions with one updating several views, an embedded demo tool plus fallback, agent-visible shared facts with separate permissions, honest provenance/stale/UNKNOWN, working phone/keyboard variant) -> a-cut-12 -> a-cut-6 -> a-pick-3 (Rylee). Static images are not prototypes. a-batch-*, a-cut-* and a-pick-3 stay BLOCKED until Rylee approves the budget.
