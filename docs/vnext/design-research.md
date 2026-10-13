# Design research for the Worlds vNext concept exploration

> **Status:** Reference · **Verified:** 2026-10-13 · **Canonical for:** where the local design references are, and the interaction mechanics borrowed from five reactive-UI references · **Read this if:** you are about to build or review one of the 30 concepts and need a pattern to start from

**In short:** this note has three parts. (1) An index of the design references, which is partly `UNKNOWN` because the external design index is not readable on this host. (2) One section per reference listing the *mechanics* (how state is shown, how a user acts, how stale or unknown data is shown, how phone and keyboard work), not colours or skins. (3) A short list of reusable interaction patterns that the concept batches can build on.

**Source limits for this session.** There was no network access and the local design index was not readable (see Part 1). Anything about the five external references below is written from general knowledge and is **not verified against a live source in this session**. Every such claim carries a `[memory]` tag, and every source link is `UNKNOWN` until someone checks it. Do not cite those claims as fact without that check.

## Decisions in force

These are the owner's decisions. Nothing in this note may contradict them.

- 2026-10-13 Rylee comment 6153184069 on #322: the three draft directions were "too unimaginative"; a wider 30-concept exploration with working interactions comes before choosing.
- 2026-10-13 brief.md update 19:20Z: "Static images are not prototypes."
- 2026-10-13 brief.md update 19:20Z: each concept needs "three working interactions with one updating several views, an embedded demo tool plus fallback, agent-visible shared facts with separate permissions, honest provenance/stale/UNKNOWN, working phone/keyboard variant".
- 2026-10-13 brief.md Authority: protected invariants are local Memory, accessibility floors, zero-provider usability, security/consent, no invented status, and external systems keep their own authority.

`brief.md` itself is not in this repository, so these lines are quoted from the packet that commissioned this note, not re-read from the file.

The repository's own documents back the same invariants: [accessibility contract](../accessibility/ACCESSIBILITY_CONTRACT.md), [TRUE-NORTH accuracy and accessibility principles](../TRUE-NORTH.md), and [ADR-0008 front door](../adr/0008-front-door.md).

---

## Part 1 — Index of design references

### 1a. Local design index (external media library)

| Reference | Location | Status on this host | What it holds / good for |
|---|---|---|---|
| Local design index `designs/_index` | `~/code/Rylee-Bee/rylee-bee-workspace/media_files/designs/_index` | **UNKNOWN — not readable.** The path does not exist on this host (`HOME=/home/w`; no `/home/w/code` directory). | UNKNOWN. The kinds of references it holds could not be listed, so none are summarised here. |

Re-run this index on the host that holds the media library. Replace this row with the real listing, and split the row by kind (for example: screenshots, component sheets, motion references, icon sets).

### 1b. In-repo design references (readable and verified to exist)

These are the references that *are* readable from this repository. Paths are relative to this file (`docs/vnext/`).

| Reference | Path | Status | Good for |
|---|---|---|---|
| Design tokens | [`design/tokens.json`](../../design/tokens.json) | Canonical | Token file that implementations consume. Use it for any colour or spacing value in a prototype. |
| Workshop v3 design authority | [`.project/design/CURRENT.md`](../../.project/design/CURRENT.md) | Canonical current | The current implementation design authority: the "The world knows how loudly to exist" principle, approvals and reservations. |
| Screen library | [`design/screens/`](../../design/screens/) | Canonical | Today, Journal, Settings and Chat screens in both themes, narrow and desktop. Good baseline for layout and density. |
| Figma export set (0.1) | [`design/exports/0.1/`](../../design/exports/0.1/) | Canonical visual source | The export set the palette reconciliation targeted. |
| Theme pack framework | [`design/THEME_PACK_FRAMEWORK.md`](../../design/THEME_PACK_FRAMEWORK.md) | Spec; partial implementation | Pack invariants. Relevant to keeping concepts theme-neutral. |
| Companion integration | [`design/COMPANION_INTEGRATION.md`](../../design/COMPANION_INTEGRATION.md) | Canonical design plus baseline/target notes | Five residents and identity design; current standalone Chat versus target contextual chat. |
| Deviations log | [`design/deviations.md`](../../design/deviations.md) | Reference | Where the built UI departs from the design. Read before treating a screen as the truth. |
| Themes | [`design/themes/`](../../design/themes/) | Reference | Theme variants. |
| Accessibility contract | [`docs/accessibility/ACCESSIBILITY_CONTRACT.md`](../accessibility/ACCESSIBILITY_CONTRACT.md) | Canonical | The floors that every concept must meet. |
| Responsive rules | [`docs/accessibility/RESPONSIVE_RULES.md`](../accessibility/RESPONSIVE_RULES.md) | Canonical | The phone/keyboard variant requirements. |
| Screen-reader walkthroughs | [`docs/accessibility/SCREEN_READER_WALKTHROUGH.md`](../accessibility/SCREEN_READER_WALKTHROUGH.md), [`FRONT-DOOR-SCREEN-READER-WALKTHROUGH.md`](../accessibility/FRONT-DOOR-SCREEN-READER-WALKTHROUGH.md) | Canonical | Worked examples of announcing state. Use them as the test for each concept's interactions. |
| Archived spec package | `design/handoff/` | Archived | Historical 0.1 spec. Do not treat as current. |
| Archived Station concepts | tag `archive/pre-design-cleanup-2026-09-25` | Archived | Earlier layout packets. Not readable from `main`; see [docs/INDEX.md](../INDEX.md). |

### 1c. External reference readability

| Reference | Readable here? | Basis for Part 2 |
|---|---|---|
| Homarr 2.0 | UNKNOWN — no network, no local copy | `[memory]` only |
| Appsmith | UNKNOWN — no network, no local copy | `[memory]` only |
| Dashy | UNKNOWN — no network, no local copy | `[memory]` only |
| Home Assistant custom cards | UNKNOWN — no network, no local copy | `[memory]` only |
| MCP ext-apps | UNKNOWN — no network, no local copy | `[memory]` only |

---

## Part 2 — Mechanics by reference

Each section lists the mechanics worth reusing. Columns from the packet: how state is surfaced, how a user acts, how staleness or unknown is shown, and how phone and keyboard work. Anything not known is written `UNKNOWN`. Source links are `UNKNOWN` throughout because none could be checked.

### 2.1 Homarr 2.0

*Self-hosted dashboard built from tiles (widgets) that pull live data from integrations.* `[memory]`

- **Mechanic — tiles as live views of an external service.** Each widget is a view over one integration's data; the dashboard does not own the data. Reuse for concepts where a tile shows a fact that belongs to another system (external systems keep their own authority).
- **Mechanic — edit mode as a separate state.** Layout changes happen in an explicit edit mode, not on the live view. Reuse for "arrange the world" concepts so that a user cannot move things by accident.
- **How staleness or unknown is shown:** UNKNOWN in detail. `[memory]` suggests per-widget error states, but the exact copy and whether a last-updated time is shown were not verified.
- **Phone / keyboard:** UNKNOWN.
- **Source link:** UNKNOWN.

### 2.2 Appsmith

*Low-code internal-tool builder: widgets are bound to queries and data through expression bindings.* `[memory]`

- **Mechanic — bindings as the reactive graph.** A widget's value is an expression over other widgets and query results; when a source changes, dependent widgets update. Reuse for "one interaction updates several views": the concept must show that a single action changes more than one view, with the dependency explicit.
- **Mechanic — queries as explicit actions with run state.** Data fetches are named, run on demand or on page load, and have a visible running and error state. Reuse for "embedded demo tool plus fallback": a demo tool is a named action with a visible result, and the fallback is what shows when it fails.
- **Mechanic — visibility and disabled bindings.** Controls can be hidden or disabled by expression. Reuse to show when an action is not currently allowed (for example, an agent-only permission) rather than hiding the reason.
- **How staleness or unknown is shown:** loading and error indicators on queries `[memory]`. Whether an explicit "stale" marker exists was not verified.
- **Phone / keyboard:** UNKNOWN for the phone layout. Keyboard focus behaviour was not verified.
- **Source link:** UNKNOWN.

### 2.3 Dashy

*Self-hosted start page configured in a file; sections of links and widgets, with optional status checks.* `[memory]`

- **Mechanic — status checks as a visible per-item indicator.** Items can be polled for reachability and show a status marker. Reuse for "agent-visible shared facts": a fact about an external service carries its own checked-at time and status, separate from the item itself.
- **Mechanic — configuration as data.** The layout is a document that the user edits, not a drag-only UI. Reuse for concepts where the layout is a shareable, reviewable artefact.
- **How staleness or unknown is shown:** the status marker `[memory]`; whether an explicit unknown state exists was not verified.
- **Phone / keyboard:** UNKNOWN.
- **Source link:** UNKNOWN.

### 2.4 Home Assistant custom cards

*Lovelace cards: a dashboard of entity cards, each subscribed to live entity state, with configurable tap and hold actions.* `[memory]`

- **Mechanic — states are first-class, including `unavailable` and `unknown`.** Entities carry an explicit state, and the card renders the non-normal states distinctly rather than showing a stale number. This is the closest match to the packet's "honest provenance/stale/UNKNOWN" requirement. Reuse directly: every fact a concept shows carries one of an explicit set of states.
- **Mechanic — tap, hold and double-tap actions declared per item.** Actions are declared as data (for example `tap_action`, `hold_action`) so that one card gives three distinct interactions. Reuse for "three working interactions": a single object exposes primary, secondary and tertiary actions.
- **Mechanic — visual editor plus YAML.** A card can be edited through a form or as raw config, and both produce the same result. Reuse as the "embedded demo tool": a visual tool plus the underlying config.
- **How staleness or unknown is shown:** `unavailable` and `unknown` as distinct rendered states `[memory]`. The exact visual treatment was not verified.
- **Phone / keyboard:** the dashboard reflows into a single column on narrow screens `[memory]`. Keyboard behaviour of custom cards varies by card and was not verified.
- **Source link:** UNKNOWN.

### 2.5 MCP ext-apps

*An extension to the Model Context Protocol in which a tool can return an interactive UI resource that the host renders, usually in a sandboxed frame, and that talks back to the host.* `[memory]` The project was moving quickly when this was written, so the exact names and fields must be checked.

- **Mechanic — tool result paired with a UI surface.** An agent's tool call returns both data and a renderable view of that data. Reuse for "agent-visible shared facts": the agent and the user see the same fact, and the view is the agreed rendering.
- **Mechanic — host-mediated calls back into tools.** The embedded UI does not call services directly; it calls back through the host, which can check permission. Reuse for "separate permissions": a concept's interactive surface asks the host for an action, and the host decides.
- **Mechanic — sandboxed embedding.** The UI runs isolated from the host page. Reuse as the security boundary for any embedded demo tool.
- **How staleness or unknown is shown:** UNKNOWN. Not verified.
- **Phone / keyboard:** UNKNOWN. The host decides how an embedded view is sized and focused, and this was not verified.
- **Source link:** UNKNOWN.

---

## Part 3 — Reusable interaction patterns for the concept batches

Each pattern names the mechanic it comes from and the packet requirement it helps meet. A concept may use several.

1. **Shared fact, many views.** One fact is stored once and rendered in several views; one action updates all of them. *From Appsmith bindings.* Meets "one updating several views".
2. **Explicit action with run state.** Every action shows running, done and failed states in place. *From Appsmith queries.* Meets "three working interactions".
3. **Primary, secondary, tertiary per object.** Each object exposes tap, hold and double-tap style actions declared together. *From Home Assistant card actions.* Meets "three working interactions" on one object.
4. **Explicit state set for every fact.** Each fact shows one of a fixed set of states (live, stale, unavailable, unknown), never a blank. *From Home Assistant entity states.* Meets "honest provenance/stale/UNKNOWN".
5. **Checked-at and source on every external fact.** A fact about an external system shows where it came from and when it was last checked. *From Dashy status checks and Homarr integration tiles.* Meets "external systems keep their own authority".
6. **Edit mode separate from view mode.** Rearranging is an explicit mode, so the live view cannot be changed by accident. *From Homarr edit mode.* Meets "no invented status" by keeping layout distinct from data.
7. **Embedded demo tool with a fallback.** A working tool sits in the concept, with a plain fallback view shown when the tool is unavailable or disabled. *From Appsmith query failure states and MCP ext-apps sandboxed views.* Meets "embedded demo tool plus fallback".
8. **Host-mediated action with a permission check.** An embedded surface requests an action; the host checks permission and records the result. *From MCP ext-apps host-mediated calls.* Meets "agent-visible shared facts with separate permissions" and "security/consent".
9. **Visual editor and config are one artefact.** A form and raw config edit the same underlying data, and both are shown. *From Home Assistant card editor.* Supports review and agent authoring.
10. **Disabled with a reason, not hidden.** A control that is not allowed is shown disabled with its reason. *From Appsmith visibility and disabled bindings.* Meets the accessibility floor, since a hidden control gives no explanation.
11. **Single column reflow for phone.** Multi-column views collapse to one column on narrow screens with the same order of actions. *From Home Assistant dashboard reflow.* Meets "working phone/keyboard variant"; check against [RESPONSIVE_RULES](../accessibility/RESPONSIVE_RULES.md).
12. **Keyboard reaches every action the pointer reaches.** Each interaction in patterns 2 to 10 must be operable from the keyboard alone, with a visible focus. *Requirement from the accessibility contract, not from a reference.* Check each concept against [the accessibility contract](../accessibility/ACCESSIBILITY_CONTRACT.md).
13. **Local-first fallback when a provider is absent.** Each concept must still show its state and actions with no external provider configured. *From the zero-provider usability invariant.* Meets "zero-provider usability".
14. **Config as a reviewable document.** Layout and settings are stored as a plain document that can be read, diffed and shared. *From Dashy's configuration model.* Supports the agent-visible review path.

Patterns 1 to 14 are a starting list, not a finished design. Each one must be tested as a working interaction in a prototype. A static picture of a pattern does not satisfy the owner's decision that "static images are not prototypes".

## Open items

- Re-index the external design media library on a host that has it; replace the `UNKNOWN` row in 1a.
- Verify every `[memory]` claim in Part 2 against the live project, and add real source links.
- Check the current MCP ext-apps specification, since it was changing quickly.
- Fill in the `UNKNOWN` phone and keyboard entries for each reference.
