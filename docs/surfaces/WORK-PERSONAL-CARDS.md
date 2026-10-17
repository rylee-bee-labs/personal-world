# Work and Personal spaces, and cards for things you're curious about (spec)

> **Status:** spec, 2026-10-17. Exploratory design input; it authorizes no build or deploy. · **From:** Rylee via Sol
> relay #628 · **Builds on:** ADR-0008 (front door), ADR-0007 (scoped credentials), the classification rules in
> `docs/ARCHITECTURE.md` · **Sibling:** agent-platform `docs/plans/2026-10-17-lab-watch.md`

**In short:** Worlds gets two spaces, **Work** and **Personal**. A card is a *thing* Rylee is curious about or
responsible for. The space is enforced where the data and credentials live, not by a UI filter. Nothing in ADR-0008
changes: the skeleton stays `Home · Connect · Memory · Settings`, and the chain stays
`Provider → Request → Mapping → Card → Board`.

## The principle

"The estate exists to protect my curiosity, not manage it." A card holds human meaning: "Ursula K. Le Guin
reissues", "the Mending Shift", "why does my DNS drop at night?". It never holds provider or API plumbing. That stays in
Connect, as ADR-0008 already says.

## Spaces: one field, enforced in four places

Every configuration file and record gets `space: work | personal`: providers, requests, mappings, cards, boards,
secret references, and Memory records. A file without one defaults to `personal`, the stricter side.

| Where | How the separation is enforced |
|---|---|
| **Credentials** | Secret references resolve only inside their space's broker path (`worlds/work/*`, `worlds/personal/*` under ADR-0007's scoped-credential broker). A request can reference only a secret from its own space; `framework validate` rejects anything else. |
| **Data scope** | The repository layer filters by space before anything reaches a view, export or agent. It's the same pattern as `classification.py` (field metadata, not folders), and space sits alongside `world/private/secret` rather than replacing it. |
| **Search** | One index per space. Searching across both is an explicit per-query choice by Rylee in the UI. It's never the default, and agents can never do it. |
| **Agent context** | An agent binding carries exactly one space. Context assembly refuses records from the other space, and leakage tests in the style of `TestSettingsExportSafety` prove it. |

Moving a card between spaces is a deliberate action, not a drag. It shows what changes (credentials, watch,
bindings), drops bindings that don't belong in the new space, and asks Rylee to confirm.

Estate tooling (`lab watch`, Project Home, Sol) may see **Work** cards whose providers are estate systems. It never
reads **Personal** data.

## A card

```yaml
# cards/le-guin-reissues.yaml
space: personal
subject: Ursula K. Le Guin reissues          # human words, her own
kind: author                                 # project | author | game | question | system | place | other
board: reading
pinned: false
view: list                                   # ADR-0008 Tier-1 views
request: bookshop.author-search              # plumbing lives in Connect, referenced by id
watch:                                       # optional
  when: new-item                             # same predicate names as lab watch
  every: 1d
  expires: never                             # long-lived: shown under "watching" in Settings
  notify: quiet                              # quiet | digest | ping
```

**Simple actions:** add (from a sentence: "keep an eye on…" suggests a kind and a request), move, reorder, pin, hide
and configure. All of these round-trip through the YAML (ADR-0008 rule 5).

**Hide, mute and stop watching are three different things:**

| Action | On the board | Still refreshed | Notifies |
|---|---|---|---|
| Hide | no; listed under "Hidden", still searchable | yes | no |
| Mute | yes | yes | no |
| Stop watching | yes, marked "not watching · last seen <time>" | no | no |

## The optional watcher

A card's watch is a Worlds request on a schedule, plus a change predicate. It uses Worlds' own request runner and
its existing TTL and stale rules: one provider failing shows `stale` with the last-good time and never blanks a board.
It borrows `lab watch`'s vocabulary (`when`, `expires`, `key`, last observation, UNKNOWN on unreachable) so both read
the same way, but no estate process runs a Personal watch. Rules:

- Silence when nothing changed. A notification is information and never authorizes an action (ADR-0008 rule 4).
- A watch on a write-capable request is impossible: watches only read.
- Every long-lived watch is listed in one place (Settings → Watching), with pause and stop.

## Visual direction (Mockup)

This matches the Project Home R1 direction Rylee picked on 2026-10-17: storybook sci-fi, **illustrated tiles** and
**gentle motion**.

- **Two places, not two filters.** Work is a lantern-lit workshop deck, and Personal is a quiet night garden under
  stars. The space switch at the top of Home is a word plus a small painting, never colour alone.
- **Card tiles by kind:** a small planet for a project, an open book with a comet tail for an author, a tiny rocket for
  a game, a lantern for a question, a satellite for a system. Same tile shape everywhere (ADR-0008 rev 3.1:
  state · one number · one line · source · freshness).
- **Gentle motion:** a soft glint when a watched card changes and a slow twinkle in the empty state. Both are off
  under reduced motion.
- **"Nothing new" is a lovely, calm state,** not an empty error.

## Next (when Rylee wants it built)

1. An ADR ("spaces"), written from this spec, because it adds a field to every config schema and a rule to the broker.
2. Spaces in `framework validate` plus leakage tests, before any UI.
3. An OpenDesign mockup of Home with both spaces, judged and shown in a gallery review session.
