# Connect contract — vnext (issue #323)

> **Status: MOCKUP.** Nothing in this document is implemented. MOCKUP ≠ PROTOTYPE ≠ APPROVED.
> It describes a *shape*, and every shape below is a proposal until an issue says otherwise.
> No route here exists in the running application, no field below is stored, and no security test
> below is written. Where this document describes today's behaviour it cites the code; where it
> describes the future it says so in the tense it is written in.
>
> **Date:** 2026-10-13 · **Issue:** #323 · **Reads with:** [ADR-0008 — the front door](../adr/0008-front-door.md),
> [C1–C9 contracts](../rebuild/CONTRACTS.md), [product direction](../../.project/PLAN.md),
> [PROVIDERS](../PROVIDERS.md), [ROOMS](../ROOMS.md).
>
> **Also:** no new backend is proposed. See §7 for why, quoting #321's fit test.

## 0. The decisions this contract obeys

Quoted, not paraphrased:

- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 issue #323: "Every field/endpoint must have a named consumer, a reason to exist and a security test." "Prefer normal providers exposing their own API, not bundling a new Sonarr/GitHub/Project Home implementation in Worlds."
- 2026-10-13 issue #321: "A new backend is allowed if justified by fit, not built merely because 'new' sounds better." "Credential *references* only, scoped access and no secret value round-trip into the browser."
- 2026-10-13 issue #319: "Worlds owns meaning. Providers own mechanics." External systems keep their own authority; Project Home owns its own consequential operations; Worlds shows/links, never mints competing authority.

Two consequences run through everything below:

1. **The vocabulary is provider-neutral.** The contract knows about *capabilities, fields,
   freshness and authority*. It does not know what a Sonarr, a GitHub or a Project Home is.
   A provider that ships an OpenAPI document and a provider that speaks `room/0` arrive through
   the same six verbs (§1) and differ only in the `dialect` string on the descriptor.
2. **Worlds never becomes a second authority for an external system.** Every consequential
   operation ends in an approval that the *governing* system issued, and `revoke` deliberately
   reports `external_state_untouched: true` — Worlds can drop its own reference, never the
   external system's own token.

## 1. The six verbs

A connection is registered once and then goes through six owner-initiated steps. Each step is a
separate endpoint, each step's output is durable, and each step is independently re-runnable.

```text
discover ──▶ register ──▶ describe-capabilities
                │
                ├──▶ test ──▶ transform ──▶ publish
                │
                └──▶ propose-action ──▶ approve (Worlds | Project Home) ──▶ execute
                                         │
                              disconnect ─┴─▶ revoke
```

- **discover** — one bounded, read-only probe that returns a *candidate* capability list. Writes nothing.
- **register** — persist a provider, its credential **reference** and the capabilities the owner kept,
  through the existing config store, so the YAML files stay canonical truth ([C1](../rebuild/CONTRACTS.md#c1-config-files-canonical-truth)).
- **describe-capabilities** — the stored, typed capability description. No network.
- **test** — one bounded read through the confinement seam; returns `status` and `field` values
  with `source` and `freshness`. No network for the transform half.
- **transform** — turn a tested field set into a Worlds card draft (`status | field | card`).
  Pure: no network, no persistence.
- **publish** — *selective*: choose exactly which capability field keys land on Home, on which board.
- **propose-action** — describe a consequential operation the owner wants callable; Worlds records a
  proposal and an approval link, and stops.
- **approve / execute** — the existing C3 lifecycle. Untouched by this document.
- **disconnect / revoke** — stop showing, then drop the reference.

## 2. The typed contract

Types are written in a TypeScript-flavoured notation for readability. They are MOCKUP notation;
the implementation would be Pydantic models beside `src/personal_world/worlds/models.py`, because
that is where `schema_version: 1`, `extra="forbid"` and the id patterns already live.

```ts
// ---------------------------------------------------------------- identity
type ContractVersion = "connect/v1";
type ConnectionId = string;      // the C1 provider id: ^[a-z0-9][a-z0-9-]{0,62}$
type Dialect = "http-json" | "openapi-3" | "room-0";
type Authority = "worlds" | "external";
type SourceState = "healthy" | "needs_attention" | "degraded" | "unavailable"
                 | "stale" | "unknown" | "not_configured";
type Freshness = "current" | "stale";

// ------------------------------------------------------- discover / register
interface RegisterRequest {
  contract: ContractVersion;
  connection_id: ConnectionId;
  display_name: string;                    // the owner's words; never the vendor's product name
  dialect: Dialect;
  destination: { base_url: string; path_prefix?: string; network?: { lan: boolean } };
  credential?: CredentialReference;        // a REFERENCE. never a value.
  capabilities: Capability[];              // the ones the owner kept from discover
  principal?: PrincipalBinding | null;     // only when the external system requires one
  publish_target?: { board: string; publish: string[] };   // optional; may be empty
}

interface ConnectionDescriptor {
  contract: ContractVersion;
  connection_id: ConnectionId;
  display_name: string;
  vendor_label: string | null;             // shown, never adopted as Worlds vocabulary
  dialect: Dialect;
  authority: Authority;                    // who governs consequential operations
  principal: PrincipalBinding | null;
  credential: CredentialReference | null;
  capabilities: Capability[];
  published: PublishSelection;
  etag: string;                            // sha256 of the file bytes; config_store.py:445
  updated_at: string;                      // ISO-8601
}

// ------------------------------------------------ describe-capabilities
interface Capability {
  capability_id: string;                   // ^[a-z0-9][a-z0-9-]{0,62}$ inside the connection
  label: string;                           // the owner's words for this thing
  meaning: { concept: string; short: string; full?: string };   // WORLDS owns meaning
  reads: ReadSpec[];                       // bounded, read-only
  provenance: "owner" | "discovered" | "provider_declared";
  verified: boolean;                       // a foreman checked the shapes read-only against a real service
}

interface ReadSpec {
  operation: string;                       // stable within the capability
  method: "GET" | "HEAD";                  // read-only by construction; see Ledger §4.1
  path: string;                            // relative; validated at save, never at first render
  query?: Record<string, string>;          // closed template set only
  bounds: { timeout_s: number; max_bytes: number; ttl_s: number };
}

// ------------------------------------------------------ credential reference
interface CredentialReference {
  ref: `env:${string}` | `vault:${string}` | `file:${string}`;
  auth: "none" | "bearer" | "header" | "basic" | "cookie_session" | "password_grant";
  header_name?: string;                    // may not be a framing or forwarding header
  scope: string[];                         // capability_ids this credential may be sent to — nothing else
  rotated_at?: string;                     // owner-supplied hint; Worlds never guesses it
}

// ------------------------------------------------------------- test result
interface TestRequest {
  capability_id: string;
  operation?: string;                      // defaults to the capability's only/first read
  assertions?: { status?: number; path?: string; exists?: boolean; is_list?: boolean; equals?: Json }[];
}

interface FieldValue { key: string; text: string; raw?: Json }   // missing is {text:"unknown"}, never 0

interface TestResult {
  contract: ContractVersion;
  capability_id: string;
  source: {
    connection_id: ConnectionId;
    request_id: string;                    // the C1 request this ran as
    method: "GET" | "HEAD";
    path: string;
    status_code: number | null;
    duration_ms: number | null;
    error_class: ErrorClass | null;        // the C2 vocabulary, unchanged
    note?: string;                         // redacted, ≤300 chars, never a body
  };
  freshness: Freshness;
  state: SourceState;
  observed_at: string;                     // ISO-8601
  fetched_at: string;
  last_good_at: string | null;
  values: Record<string, FieldValue>;
  meter?: { type: string; text_equivalent: string } | null;
  truncated: boolean;
}

// -------------------------------------------------------------- transform
interface CardDraft {
  card_id: string;
  connection_id: ConnectionId;
  capability_id: string;
  title: string;
  meaning: { concept: string; short: string; full?: string };
  fields: { key: string; label: string; format: string; unit?: string }[];
  status_map?: { path: string; healthy: Json[]; needs_attention: Json[]; mode: "first" | "all" | "any" };
  source_state: SourceState;
  freshness: Freshness;
  source: TestResult["source"];
}

// ------------------------------------------------- selective publish
interface PublishSelection {
  board: string;
  published: string[];                     // exactly the field keys that land on Home
  hidden: string[];                        // capability ids that stay in Connect and go no further
  published_at: string | null;
  published_by: "owner" | null;            // never an agent token
}

// ------------------------------------------------- independently-governed action
interface ActionProposal {
  contract: ContractVersion;
  proposal_id: string;
  connection_id: ConnectionId;
  capability_id: string;
  operation: string;
  label: string;
  effect: "write";                         // a proposal is always for a consequential operation
  scope: string;
  idempotency: "required" | "optional" | "none";
  exposed: false;                          // an agent token is never a caller of an adopted external write
  approval: ApprovalLink;
  state: "proposed" | "adopted" | "refused" | "expired";
}

type ApprovalLink =
  | { kind: "worlds"; authorization_id: string; approve_at: string }
  | { kind: "project_home"; room_url: string; action_ref: string; record_at: string };

// ------------------------------------------------------- disconnect / revoke
interface DisconnectResult {
  disconnected: true;
  published_cleared: string[];             // card ids removed from Home
  caches_dropped: string[];               // last-good snapshots and back-off
  credentials_retained: string[];         // the ref stays; nothing is sent
  pending_authorizations: string[];       // listed, never silently cancelled
}

interface RevocationResult {
  revoked: true;
  credential_ref_removed: string | null;  // the ref string, never its value
  held_credentials_purged: number;        // in-process sessions/tokens dropped
  config_deleted: string[];               // provider/request/card/action ids removed
  external_state_untouched: true;         // Worlds cannot revoke the external system's own token
}
```

## 3. Endpoints

Every endpoint is owner-only with CSRF, as a write in the front door is today
(`src/personal_world/worlds/connect_routes.py:120`). "Network" says whether the call leaves the box.

| # | Method + path | Consumer (named) | Reason to exist | Network | Security test |
|---|---|---|---|---|---|
| 1 | `POST /api/connect/v1/discover` | Connect UI, Discover step (today `/api/connect/try`, `ui/src/fd/api.ts:180`) | Return a candidate capability list before anything is saved | 1 bounded read | Untrusted document bounds (size/depth/nodes/operations); GET+HEAD only; `servers[]` ignored in favour of a synthetic placeholder (`recipes.py:163`); unmapped auth leaves `needs_auth: unknown`, never a silent `none` (`recipes.py:328`) |
| 2 | `POST /api/connect/v1/connections` | Connect UI, "Keep these"; `framework.py:126` config validation | Persist provider + credential ref + kept capabilities through `ConfigStore` so YAML stays canonical | no | Id validated *before* a path is built (`config_store.py:119`); placeholder `base_url` refused (`recipes.py:589`); `If-None-Match: *` on create, real etag on update |
| 3 | `GET /api/connect/v1/connections` | Connect UI, connection list; Connect empty state (`ui/src/fd/Connect.tsx:711`) | Show what is connected without opening each file | no | Owner-only; descriptor is a reference-only projection of C1 files, so it cannot leak more than the config already holds |
| 4 | `GET /api/connect/v1/connections/{id}` | Connect UI, connection detail | One read of the stored descriptor | no | Owner-only; `Cache-Control: no-store` |
| 5 | `GET /api/connect/v1/connections/{id}/capabilities` | Connect UI, capability list; `GET /api/boards/home` for what is published | The stored typed capability description, no network | no | Owner-only; no capability is callable from here — descriptions are not authority |
| 6 | `POST /api/connect/v1/connections/{id}/test` | Connect UI, "Test this"; the recipe `verified: false` gate (`recipes.py:69`) | One bounded read → `status`/`field` values with `source` + `freshness` | 1 bounded read | Read effect only — a write is refused 422, as `connect_routes.py:115` does; rate-limited 10/min (`connect_routes.py:34`); sample scrubbed before it is serialized (`connect_routes.py:191`); `PW_*`/`OIDC_*`/`BOOTSTRAP` refs refused (`connect_routes.py:37`) |
| 7 | `POST /api/connect/v1/connections/{id}/transform` | Connect UI, "Make a card" | Turn a `TestResult` into a `CardDraft`; pure, so the owner can iterate with no network | no | Sample size and depth caps (`connect_routes.py:36,159`); a JSONPath subset with no filters or scripts, validated at save (`models.py:26`); no write ever happens here |
| 8 | `POST /api/connect/v1/connections/{id}/publish` | Connect UI, "Show this on Home"; `production.py:91` `home_extra` | **Selective** publish: pick the exact field keys and board; everything else stays in Connect | no | `published` is an allow-list, never a default-on wildcard; a hidden capability is absent from `GET /api/boards/home` payloads entirely; an agent token is refused; publishes only a `CardDraft` the owner already saw from a `test` |
| 9 | `POST /api/connect/v1/connections/{id}/actions` | Connect UI, "Make this callable"; `actions_routes.py:60` | Propose a consequential operation and get an approval link; sends nothing | no | `exposed: false` is not settable here; `approval: never` on a write refused unless the owner's explicit waiver is in config (`config_store.py:355`); `idempotency: required` for an external write |
| 10 | `POST /api/connect/v1/connections/{id}/actions/{proposal_id}/adopt` | Connect UI, action row | Turn a proposal into a callable C1 request + action | no | Sends nothing (`room0.py:604`); the request it creates carries no body; an id a provider could not have minted is refused 400 (`room0.py:546`) |
| 11 | `POST /api/connect/v1/approvals/{authorization_id}/approve` | Home → Needs a look → approve (`production.py:82`); `actions_routes.py:71` | The one Worlds approval path, step-up required | no | Step-up inside 120 s or refused (`dispatcher.py:262`); an agent token never approves; a `project_home`-governed connection cannot be approved here at all |
| 12 | `POST /api/connect/v1/approvals/{authorization_id}/record-external` | Project Home room, on its own approve (`room_routes.py:72`) | Record that the *governing* system approved; Worlds does not re-decide it | no | Accepts only a Project Home approval id, recorded as `authority: project_home:<id>` (`dispatcher.py:289`); a `worlds`-governed connection refuses this endpoint |
| 13 | `POST /api/connect/v1/connections/{id}/disconnect` | Connect UI, "Stop showing this" | Stop publishing and drop caches without deleting config | no | Refuses while an authorization is pending unless the owner names them; never touches the external system |
| 14 | `POST /api/connect/v1/connections/{id}/revoke` | Connect UI, "Remove this connection" | Drop the credential reference, purge held credentials, delete the config objects | no | Config deletes are refused while anything still references the object (`config_store.py:358`); held credentials purged (`confinement.py:298`); reports `external_state_untouched: true` and tells the owner to revoke at the provider |

Endpoints 11 and 12 are *proposals to reuse*, not new machinery: both already exist
(`actions_routes.py:71` and `room_routes.py:71`) and are listed here so the Connect flow has one
place to point at. Whether they move under `/api/connect/v1/` is a §6 question, and the default
answer is **no**: a path that already works is not moved for tidiness.

## 4. Field ledger

The rule from #323, applied field by field. `consumer` is a named thing in this repo or a named
screen, not "the API".

### 4.1 `ConnectionDescriptor`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `contract` | Connect UI, migration check | Says which generation of the contract wrote this | Rejects anything but `connect/v1`; never guessed from a missing value |
| `display_name` | Home board item title; Connect list | Worlds owns meaning, so the owner names it, not the vendor | Length-bounded; never rendered as markup; a name that reads as a URL is not turned into a link |
| `vendor_label` | Connect detail, for the owner's own recognition | The owner's "what is this thing called" | Shown, never used as an id, never used as a vocabulary term, never matched against |
| `dialect` | Dispatch, at the boundary only | Chooses which read adapter and which auth exchange applies | A closed enum; an unknown dialect is refused at register, not defaulted |
| `authority` | `actions_routes` approve/record split | Decides *who* may approve — the single most consequential field here | `external` on a provider whose operations the owner cannot approve in Worlds; defaults closed (`models.py:203`) |
| `principal` | The sender's identity header, for `room-0` | Some systems require Worlds to say who it speaks for | Pattern-validated at save and again at send (`confinement.py:606`); never taken from a request header |
| `credential` | The sender only | The provider needs a credential; the browser must never see one | Reference-only; a value anywhere in the body is refused by `extra="forbid"` because no model has a field for it |
| `capabilities` | Connect capability list; `describe-capabilities` | The typed thing a connection *has* | Every entry is read-only; a write-shaped capability cannot be expressed here (see §4.5) |
| `etag` | The owner's next edit | Optimistic concurrency, as C1 requires | 409 on mismatch; `If-Match: *` refused with 428 |
| `updated_at` | Connect "changed" hint | The owner can tell stale config from current | Never used to decide freshness — that comes from `fetched_at`/`last_good_at` |

### 4.2 `CredentialReference`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `ref` | `secrets.resolve_secret_ref` at send time | Names a secret without holding it | Regex-bounded at save (`models.py:15`); Worlds' own `PW_*`/`OIDC_*`/`BOOTSTRAP` names refused for unsaved providers (`connect_routes.py:101`) |
| `auth` | The sender's credential exchange | *How* to present it | Closed enum; `none` is never chosen silently for a scheme the discovery step did not understand |
| `header_name` | The sender | Names a non-standard header | May not be a framing, forwarding or `sec-` header (`models.py:53`); re-checked at send (`confinement.py:246`) |
| `scope` | The sender, before it dials | **Scoped access**: this credential may only be sent to the listed capabilities | A capability outside `scope` is `auth_failed` before any socket opens — no DNS, no bytes |
| `rotated_at` | Connect detail | The owner's own reminder; Worlds never auto-rotates | Read-only display; never used to decide whether a credential is still valid |

### 4.3 `TestResult`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `source.connection_id`, `request_id` | History rows; the receipt; `Home → needs a look` | Which request produced this | Must match a stored read request; an unsaved id is refused |
| `source.method`, `path` | The detail view's evidence strip | Which call it was | Path re-validated at send (`confinement.py:201`); `..`, `%2e`, `\` and control characters refused |
| `source.status_code`, `duration_ms`, `error_class` | Needs-a-look grouping; degraded-mode copy | Why it failed, in the C2 vocabulary | Classified, never free text from the provider |
| `source.note` | The same strip | A short human reason | Scrubbed against every live secret, then cut to 300 chars (`connect_routes.py:176`) |
| `freshness`, `fetched_at`, `last_good_at` | Home freshness text; C5 accessibility copy | Availability and freshness are separate things | Last-good values are *always* labelled `stale`; a card is never shown as current on last-good data |
| `state` | Needs-a-look grouping; whole-world strip | The Play-Nice word | Bounded vocabulary; `not_configured` is never a Home row (`CONTRACTS.md` C6) |
| `values` | The card, the accessibility name, agent answers | What the person actually sees | Missing is `{text:"unknown"}`, never 0; an empty list is `none`, never unavailable; every path is the validated JSONPath subset |
| `meter` | The card's meter + `text_equivalent` | A number with a text equivalent | An unresolvable source is **omitted**, never 0; only numbers/booleans/strings survive, ≤200 items |
| `truncated` | Connect's honest-copy path | Tells the owner the sample is partial | Set whenever a cap fired; a truncated sample can never become a published field |

### 4.4 `PublishSelection`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `published` | `GET /api/boards/home` (`production.py:91`) | Exactly which field keys are shown | An allow-list. Empty means nothing is published. A key not produced by a `test` is refused |
| `hidden` | Connect detail | Makes "kept but not shown" visible rather than silent | A hidden capability appears in no board payload, no agent answer and no receipt |
| `board` | The home board the card joins | Where it lands | Must be an existing board; a card with no board is `not_configured` |
| `published_at`, `published_by` | History | When and by whom | `published_by` is always `owner`; an agent token cannot set it |

### 4.5 `ActionProposal`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `effect` | `_check_action_policy` | A proposal is always for something consequential | Literal `"write"`; it cannot be constructed as a read |
| `scope` | Agent answer filtering; the dispatcher | Which capability the action belongs to | A caller's scope is checked against this before an authorization is even requested |
| `idempotency` | The dispatcher's single-attempt rule | A retry must be safe or must not exist | A write with no idempotency key is refused, not retried |
| `exposed` | Agent visibility | An adopted external write is **not** an agent tool | Literal `false`; not settable from any endpoint in §3 |
| `approval` | Home → approve; the external room | Names *who* must approve and where they approve | An `authority: external` connection cannot be approved at endpoint 11 |
| `state` | Connect's action row | Proposed ≠ adopted ≠ executed | `adopt` sends nothing; `execute` needs a consumed authorization |

### 4.6 `Disconnect` / `Revoke`

| Field | Consumer | Reason to exist | Security test |
|---|---|---|---|
| `published_cleared` | Home | The cards leave, and the owner sees exactly which | Delete refused while another object still references the card (`config_store.py:358`) |
| `caches_dropped` | Connect detail | A disconnected connection must not keep answering from a last-good snapshot | Same lock `snapshot()` uses, so a disconnect cannot race a fetch |
| `credentials_retained` | Connect detail | Disconnect is reversible; the owner can see what stayed | Never printed as a value; a `ref` string only |
| `pending_authorizations` | Connect detail | A disconnect does not silently cancel a pending approval | Listed and named; cancelling is a separate, explicit owner act |
| `held_credentials_purged` | Connect detail; `confinement.py:298` | A revoked connection must not keep a live session in process memory | Count only; the token itself is never echoed, logged or returned |
| `external_state_untouched` | Connect detail | Worlds cannot revoke someone else's token — saying so is the honest answer | Literal `true`; the UI must tell the owner to rotate at the provider |

## 5. Threat and authority

### 5.1 Threats

| Threat | Where it enters | Control in this contract | Already in code at | Security test to write |
|---|---|---|---|---|
| SSRF into loopback / RFC1918 / CGNAT, or DNS rebinding between resolve and dial | 1, 6, and every dispatch | One confinement seam; resolve once, classify every answer, dial a pinned IP literal | `confinement.py:136`, `:593` | "A rebinding resolver whose second answer is 169.254.169.254 sends nothing" |
| Cloud metadata theft, even for a `lan: true` provider | 1, 6 | Metadata refused unconditionally | `confinement.py:82`, `:155` | "`network.lan: true` does not unblock 169.254.169.254" |
| Borrowing a Worlds secret to call an attacker's URL | 1, 2, 6 | Unsaved providers may name a ref; Worlds' own names are refused | `connect_routes.py:37`, `:101` | "`env:PW_BOOTSTRAP_TOKEN` is refused with 422 and no request is sent" |
| A secret value round-tripping into the browser | 1, 6, 7 | References only; samples redacted *before* serialization | `connect_routes.py:191`; `production.py:88` | "A response body echoing the live token has that token replaced in the sample" |
| A credential sent to a capability outside its scope | 6, dispatch | `CredentialReference.scope` checked before dialling | new (scoping is new; the send is `confinement.py:228`) | "A capability outside `scope` fails with no DNS lookup" |
| A write smuggled through test/transform | 6, 7 | Read effect only; `effect: write` refused | `connect_routes.py:115`; `confinement.py:577` | "`POST` on the test endpoint is 422 and nothing is sent" |
| Auto-approving a consequential operation | 9, 11 | `approval: always` default; the dispatcher re-checks; an agent token is never approval | `config_store.py:349`; `dispatcher.py:262` | "A hand-edited action with `approval: never` is still refused at dispatch" |
| Hostile capability document (bomb, deep nesting, remote `$ref`, operation flood) | 1 | Bounded document; local refs only; synthetic placeholder address | `recipes.py:165`–`:217` (to be moved, §7.4) | "A 3 MB / 10 000-node document is refused with a plain message" |
| Over-broad publish leaking a provider's whole surface to Home and agents | 8 | `published` is an allow-list; `hidden` is explicit | new | "A capability not in `published` appears in no `/api/boards/home` payload" |
| Stale data shown as current | 6, 7 | `freshness` + `last_good_at` on every result | `room0.py:434`; C2 | "A failing provider's card reads `freshness: stale`, never `current`" |
| Cross-origin link injection from a provider descriptor | 8, and any `link` | Links are same-origin paths or they are dropped | `room0.py:71` (rule 6) | "`https://elsewhere.example/` in a link is dropped, not rendered" |
| Action-id collision or look-alike ids | 9, 10 | A short hash of the raw external id rides along | `room0.py:540` | "`a.b` and `a:b` and `a-b` do not collide" |
| Unbounded typed input from a provider | 9 | ≤20 choices, ≤120 chars, duplicates collapse | `room0.py:210` | "A 5000-choice descriptor yields 20 and stays a valid action" |
| Redirect escape | 6, dispatch | Never followed | `confinement.py:776` | "A 302 is `redirect_refused`, and the Location target is never dialled" |
| Decompression bomb | 6 | `identity` only; raw bytes counted against the cap | `confinement.py:809` | "A 100 KB body that expands to 100 MB is `too_large`" |
| A revoked connection that keeps working | 13, 14 | Purge held credentials; delete config; drop caches | `confinement.py:286`–`:301` | "After revoke, a second call resolves no credential and sends nothing" |
| Worlds minting authority an external system already owns | 9–12 | `authority` is recorded, not assumed; approval links point outward | `models.py:203`; `dispatcher.py:275` | "An `external`-governed connection cannot be approved at endpoint 11" |

### 5.2 Authority

Who decides what, and who never decides it. This table is the one that must survive every
future change to the contract.

| Actor | Decides | Never decides | Enforced at |
|---|---|---|---|
| **Owner** (session + step-up) | What a connection is named; which capabilities are kept; which fields are published; which actions may be adopted; whether to disconnect or revoke; approval for `worlds`-governed operations | Cannot approve on Project Home's behalf for a connection it governs; cannot bypass its own step-up | `actions_routes.py:71`; `dispatcher.py:262` |
| **Project Home** (or any external governor) | Consequential operations on connections it governs; it issues its own approval id | Does not read Worlds memory, does not become a Worlds config object, does not learn what else the owner connected | `dispatcher.py:275`; `room0.py:598`; `models.py:203` |
| **Worlds** | Meaning: labels, field selection, board membership, freshness and state words, and which capabilities exist as *descriptions* | Does not execute an externally governed operation without that system's approval; does not mint a status an external system contradicts | `production.py:82`; `config_store.py:349` |
| **The provider** | Its own mechanics: its API, its auth, its rate limits, the truth of its own data, the approval of what it governs | Does not become Worlds vocabulary; a provider-declared capability is `verified: false` until a foreman checks it | `recipes.py:69`; §4.5 `verified` |
| **Agent token** | Exposed read actions within scope, nothing else | Never approves, never publishes, never adopts, never disconnects, never revokes, never proposes | `exposed: false`; `dispatcher.py:210` |
| **A discovery document** (OpenAPI or otherwise) | Candidate capability *proposals*, each labelled with `provenance` | Never capability truth; never a credential; never an address Worlds will dial | `recipes.py:146`–`:160` |

## 6. Version and migration impact

**Version rules.** A connection descriptor declares the contract that wrote it. Within
`connect/v1`, changes are additive: a new optional field, a new `dialect`, a new endpoint. A
removal, a rename, or a change to the meaning of an existing field is `connect/v2`, and a v1
descriptor keeps being read by v1 code. There is no in-place migration of a stored connection: to
move to v2 the owner re-registers, and the old config objects stay readable until they are deleted
by hand.

| Artefact | Today | In `connect/v1` | Impact | Who feels it |
|---|---|---|---|---|
| `schema_version: 1` in every config file | Canonical truth ([C1](../rebuild/CONTRACTS.md)) | **Unchanged** | None — v1 adds no config file and changes no model | Nobody |
| Config file layout under `worlds/` | `providers/ requests/ cards/ boards/ actions/` | **Unchanged** — a connection *is* a provider plus derived requests and cards | None | Nobody |
| `Provider.kind` | `http \| room0 \| reference` | **Unchanged** — `dialect` is a Connect-level attribute, not a new kind | A third-party `kind` must not be added to make a dialect work | Owner, at register |
| `/api/connect/try`, `/api/connect/preview` | Live (`connect_routes.py:119`, `:142`) | Superseded by endpoints 6 and 7 | Kept until the UI moves; both return a `TestResult` under the new contract so the UI changes in one step, not two | Connect UI |
| Recipe install | A directory of YAML templates, installed by `recipes.install` | Replaced by endpoint 2 (register) | 16 shipped recipe directories become either importable descriptors or retired; the loader's validating half survives inside discovery (§7.4) | Owner, on the day the UI stops offering Recipes |
| OpenAPI import | `from_openapi`, a pure function with no production caller | Becomes the `openapi-3` discovery adapter | Same bounds, same refusals, same `needs_auth: unknown`; moved, not rewritten | Nobody, if the tests move with it |
| C3 action lifecycle | `request_authorization → approve → consume → dispatch` | **Unchanged** — the contract proposes, the existing lifecycle decides | None. A new backend here would be the "new because new" case #321 rules out | Nobody |
| `/api/rooms/*` | Live (`room_routes.py:16`) | **Unchanged paths**; reached in Connect through `dialect: room-0`, `authority: external` | The room flow becomes the reference implementation of an externally governed connection, not a special case | Nobody |
| `action_version` and per-file etags | Computed from action + request + provider (`config_store.py:373`) | **Unchanged** | A published edit still invalidates pending authorizations | Nobody |
| `worlds.db` tables | `authorizations`, `executions`, memory, history | **Unchanged** — no table added | None | Nobody |
| `needs_you` items | Pending authorizations + room needs (`production.py:75`) | **Unchanged** shape; a proposal with no authorization yet is not a needs-you item | A proposal is silent until someone acts on it — deliberate | Owner |
| `GET /api/boards/home` payload | Board items with display definitions, no request ids or paths | **Unchanged** — publish adds items, it does not change the payload | None | UI |

Two migrations deliberately **not** planned, and why:

- **No data migration.** `CONTRACTS.md` C1.3 fixes the rule and the owner restated it: config
  files are canonical and already round-trip. A v1 connection is readable by today's code, because
  it *is* today's config.
- **No new backend.** See §7.

## 7. Keep / adapt / replace

The calls, with the consumer evidence behind each. Evidence is `path:line` in this repository at
`786f506`.

### 7.1 `src/personal_world/worlds/config_store.py` — **KEEP**

It is the storage the new layer writes through. Changing it would be changing the canonical truth
to accommodate a design that has not been built yet.

| Consumer | Evidence |
|---|---|
| Config CRUD routes | `src/personal_world/worlds/server.py:45`, `:135`, `:291` |
| Production assembly, plus a second read-only view for redaction | `src/personal_world/worlds/production.py:86`, `:88` |
| C3 dispatcher | `src/personal_world/worlds/dispatcher.py:33`, `:154` |
| Connect workshop routes | `src/personal_world/worlds/connect_routes.py:28`, `:62` |
| Room routes | `src/personal_world/worlds/room_routes.py:11` |
| Seed config | `src/personal_world/worlds/seed.py:36`, `:46` |
| Room0 adapter | `src/personal_world/worlds/room0.py:30` |
| Recipe install | `src/personal_world/worlds/recipes.py:37`, `:567` |
| Framework validator | `src/personal_world/framework.py:44`, `:126`, `:152`, `:529` |
| Tests | `tests/worlds/test_config_store.py` (16 tests) |

**What Connect borrows, unchanged:** id validation before a path is built (`:119`), atomic writes
with `fsync` (`:425`), etag preconditions (`:276`), reference checks on save (`:326`), delete
refused while referenced (`:358`), and the computed action version (`:373`).

**What Connect must not add to it:** no new kind, no new field, no new file layout. A connection
that needs one of those is a signal the design is wrong, not that the store should grow.

### 7.2 `src/personal_world/worlds/confinement.py` — **KEEP as the only outbound seam**

Every endpoint that touches the network in §3 routes here. The fit test from #321 —
"a new backend is allowed if justified by fit, not built merely because 'new' sounds better" —
is answered by this table: the module already *is* the thing a new backend would have to be, and
it is already the tested thing.

| Consumer | Evidence |
|---|---|
| C3 dispatcher — every consequential operation | `src/personal_world/worlds/dispatcher.py:34`, `:165` |
| Connect workshop | `src/personal_world/worlds/connect_routes.py:29`, `:66` |
| Room0 client | `src/personal_world/worlds/room0.py:31`, `:289` |
| Companion | `src/personal_world/worlds/companion.py:27`, `:324`; `companion_routes.py:18`, `:26` |
| Reference provider (shares `Deadline`, `perform`) | `src/personal_world/worlds/reference_provider.py:42` |
| Runner (the C2 error vocabulary) | `src/personal_world/worlds/runner.py:35` |
| Tests | `tests/worlds/test_confinement.py` (30), `tests/worlds/test_confinement_auth.py` (40) |

**What Connect borrows, unchanged:** `classify_address` (`:136`), the metadata set (`:82`),
`build_url` (`:183`), the read-cannot-carry-a-mutating-method rule (`:577`), no redirects, no
retries, `trust_env=False` (`:770`), the raw byte cap (`:802`), the session key that includes the
whole provider identity (`:267`), and `secret_values` never reaching a note (`:554`).

**What is genuinely new, and where it goes:** `CredentialReference.scope` (§4.2) is a *decision*
before the send, so it belongs at the call sites that build a request — `dispatcher._resolve`
(`dispatcher.py:189`) and the new discover/test path — not inside the transport. A transport that
decided scope would have to know the capability graph, which is meaning, and meaning is Worlds'.

**Why not a new backend:** a replacement would have to re-prove SSRF refusal, credential
non-disclosure, the single-attempt rule and the recovery semantics, against the same tests, for no
gain in fit. That is the "new sounds better" case the owner ruled out on 2026-10-13.

### 7.3 `src/personal_world/worlds/room0.py` — **ADAPT**

The read path stays exactly as it is. What changes is that it stops being a *kind* of thing and
becomes one dialect of a provider-neutral connection.

| Consumer | Evidence |
|---|---|
| Production assembly — home items, card fallback, needs | `src/personal_world/worlds/production.py:32`, `:91`, `:93` |
| Room routes | `src/personal_world/worlds/room_routes.py:13`, `:16` |
| Tests | `tests/worlds/test_room0.py` (59 tests) |

**Keep as-is:** the last-good + back-off behaviour (`:302`, `:402`), the 2 s snapshot budget
(`:470`), same-origin link validation (`:71`), the honest `unknown` over a made-up value (`:142`),
the tone and autonomy fail-closed defaults (`:250`), the choice bounds (`:210`), the action-id
hash (`:540`), and `answer` refusing to shortcut authority (`:579`).

**Adapt, one behaviour, with a named consumer:** *selective publish*. Today every card a room
lists lands on Home (`room0.py:391` → `home_items` at `:493`). Under §4.4 the owner chooses. The
named consumer is Home itself — a room that publishes twelve cards the owner reads none of is a
"needs a look" section that never empties. Every other rule in C8 stands, and the change is
additive: an owner who wants everything keeps everything by publishing everything.

**Adapt, one authority path:** `authority` is read off the provider today through
`governed_by_project_home()` (`models.py:203`) and defaulted per kind. Under §5.2 it is an
explicit descriptor field, defaulted closed the same way. Same behaviour, one fewer inference.

**Known gap this contract exposes, not fixes:** `Dispatcher` accepts a `project_home_verifier`
(`dispatcher.py:160`) and production never supplies one (`production.py:95`). So a recorded
Project Home approval id is stored but not verified against Project Home. Endpoint 12 is where
that must be settled. Flagged here, not silently designed around.

### 7.4 `src/personal_world/worlds/recipes.py` — **REPLACE the install path, KEEP the discipline**

The evidence for the split:

| Consumer | Evidence | Verdict |
|---|---|---|
| Framework validator, which loads every shipped recipe | `src/personal_world/framework.py:46`, `:198`, `:226` | **Keep the loader's validating half** — this is a real consumer |
| Shipped recipe directories | `config/recipes/` — 16 of them | Data, not code. Retired or converted; not a reason to keep a module |
| `install()` | **No caller anywhere in `src/`** — `grep -w install` over `src/` returns 31 hits, none of them `recipes.install` | **Replace.** An unreferenced writer with a UI stub is not a consumer |
| `list_recipes()` | No caller in `src/` | **Replace** with endpoint 3 |
| `from_openapi()` | No caller in `src/`; `tests/recipes/test_openapi_import.py:13` | **Adapt** into the `openapi-3` discovery adapter, with its tests |
| Connect UI | `ui/src/fd/Connect.tsx:711` — a stub panel that says no recipes are installed | **Replace** with endpoint 2 |
| Tests | `tests/recipes/` (29 tests), `tests/worlds/test_room_recipes.py` (2) | Move with the code they cover; do not delete a test to delete a module |

The valuable half is the *discipline*, and every item below is a security control with a test
behind it: `PLACEHOLDER_BASE_URL` so no recipe ever ships a real host (`:163`), the document bounds
(`:165`–`:217`), local-`$ref`-only (`:230`), auth schemes that map to nothing becoming a TODO with
`needs_auth: unknown` rather than a silent `none` (`:298`), `_is_placeholder` refusing an install
with the placeholder address (`:561`), and the `env:` name-only secret refs (`:255`).

**What replaces it:** endpoints 1 and 2. Registering a connection writes the same objects through
the same `ConfigStore` with the same validation, so nothing is lost — the recipes' job was to
avoid hand-writing YAML, and the Connect UI does that job better and with the same guards.

**Status words:** #320 (wiring the old recipe UI) stays parked. This document does not un-park it
and does not replace it; it says the install path has no consumer, which is the evidence a
future un-parking decision needs.

## 8. Worked integration A — a generic HTTP service (read-only)

**This is a MOCKUP walkthrough, not a verified connection.** The service is a plausible homelab
HTTP service of the shape `config/recipes/gatus/` already describes. Nothing below has been run
against a live service; `verified` stays `false` until a foreman checks the shapes read-only,
exactly as `recipes.py:69` requires.

**1. discover** — the owner points at the service. One bounded read; writes nothing.

```jsonc
// POST /api/connect/v1/discover
{ "contract": "connect/v1", "connection_id": "homelab-health",
  "destination": { "base_url": "http://gatus.lan.example:8080", "network": { "lan": true } },
  "credential": null, "dialect": "http-json" }

// → 200
{ "contract": "connect/v1", "dialect": "http-json",
  "candidates": [
    { "capability_id": "check-results", "provenance": "discovered", "verified": false,
      "reads": [{ "operation": "statuses", "method": "GET", "path": "/api/v1/endpoints/statuses",
                  "bounds": { "timeout_s": 5, "max_bytes": 1048576, "ttl_s": 60 } }] }
  ],
  "skipped": [], "notes": ["base_url is a synthetic placeholder; supply the real address at register"],
  "needs_auth": "none" }
```

Consumer: Connect's Discover step. If the service publishes an OpenAPI 3.x document, the
`openapi-3` dialect produces the same shape through `from_openapi`'s bounds; every method other
than GET/HEAD lands in `skipped` with `write-like: needs an approved action`, and an auth scheme
C1 cannot map leaves `needs_auth: unknown` rather than choosing `none` for you.

**2. register** — the owner keeps one capability and names it in their own words.

```jsonc
{ "contract": "connect/v1", "connection_id": "homelab-health",
  "display_name": "Homelab checks",
  "dialect": "http-json",
  "destination": { "base_url": "http://gatus.lan.example:8080", "network": { "lan": true } },
  "credential": { "ref": "env:", "auth": "none", "scope": ["check-results"] },
  "capabilities": [{ "capability_id": "check-results", "label": "Are the checks passing?",
                     "meaning": { "concept": "homelab health",
                                  "short": "Are the homelab checks passing?" },
                     "provenance": "owner", "verified": false,
                     "reads": [{ "operation": "statuses", "method": "GET",
                                 "path": "/api/v1/endpoints/statuses",
                                 "bounds": { "timeout_s": 5, "max_bytes": 1048576, "ttl_s": 60 } }] }],
  "publish_target": { "board": "home", "publish": ["Checks", "State"] } }
```

`verified: false` survives registration on purpose. The owner has confirmed the *shapes*; nobody
has confirmed they against the live service. Merging those two words is how a status gets
overstated.

**3. test** — one read, bounded, scrubbed. `error_class` is the C2 vocabulary unchanged.

```jsonc
{ "contract": "connect/v1", "capability_id": "check-results",
  "source": { "connection_id": "homelab-health", "request_id": "homelab-health.statuses",
              "method": "GET", "path": "/api/v1/endpoints/statuses",
              "status_code": 200, "duration_ms": 41, "error_class": null },
  "freshness": "current", "state": "healthy",
  "observed_at": "2026-10-13T14:02:11Z", "fetched_at": "2026-10-13T14:02:11Z", "last_good_at": null,
  "values": { "Checks": { "key": "Checks", "text": "42" },
              "Failing": { "key": "Failing", "text": "0" } },
  "meter": { "type": "dots", "text_equivalent": "all 42 checks passing" },
  "truncated": false }
```

If the service is down, `state` becomes `unavailable`, `error_class` is `connection`, and the
`values` above come back as `last_good_at` values with `freshness: "stale"`. Missing is
`{"text": "unknown"}`, never `0`.

**4. transform** — pure, no network: `TestResult` → `CardDraft`, with the JSONPath subset paths
validated at save rather than at first render.

**5. publish (selective)** — the owner publishes `Checks` and the status word, and *not* the
per-endpoint names. The card on Home answers "are the checks passing"; the drill-down list stays
in Connect, where it is looked at on purpose. The C5 accessibility props come from the envelope
as they do for any card.

**6. propose-action** — if the owner wants "re-run the checks", it is a `write`, `scope` is
`homelab-health.check-results`, `idempotency: "required"`, `exposed: false`, and the approval
link is `{ kind: "worlds", authorization_id: … }` — because this connection is `authority:
"worlds"`. Nothing is sent at propose or at adopt; only a consumed authorization dispatches, once.

## 9. Worked integration B — Project Home room/0 (read-only)

**This is a MOCKUP walkthrough.** The wiring it describes exists; the Connect-shaped path through
it does not. What follows is what §1 looks like when `dialect` is `room-0` and `authority` is
`external`.

**The descriptor is where the two worked integrations differ, and almost nowhere else.**

```jsonc
{ "contract": "connect/v1", "connection_id": "project-home",
  "display_name": "Project Home", "vendor_label": "Project Home",
  "dialect": "room-0", "authority": "external",
  "principal": { "person_ref": "your-person-id" },
  "credential": { "ref": "env:PROJECT_HOME_ROOM_TOKEN", "auth": "bearer",
                  "scope": ["room-status", "room-cards", "room-needs", "room-actions"] },
  "capabilities": [
    { "capability_id": "room-status",  "provenance": "owner",     "verified": false },
    { "capability_id": "room-cards",   "provenance": "owner",     "verified": false },
    { "capability_id": "room-needs",   "provenance": "owner",     "verified": false },
    { "capability_id": "room-actions", "provenance": "discovered", "verified": false }
  ] }
```

`your-person-id` is the placeholder `config/recipes/project-home/provider.yaml` ships, and it is
installed as written: Worlds speaks for one named person, validated at save and again at send
(`confinement.py:606`), and never as a free request header. The owner replaces it in their own
config; nothing in Worlds guesses it.

**1. discover** reads `/room` and the three lists, through the confinement seam, with the same
caps as any other connection. A room that does not answer `contract: room/0` is `malformed` →
`degraded`, never silently empty (`room0.py:384`).

**2. register** writes the provider only. **No request, card or action files are invented** — the
room maps its own cards through C8 (`CONTRACTS.md` C8). This is the `status: room` recipe's one
honest idea, and it survives into the contract unchanged.

**3–4. test / transform** map a room card to `TestResult` with `source.request_id` of the form
`<provider>.room` and `source.path` of `/room/cards`, then to a `CardDraft`. Descriptor status
maps healthy→healthy, degraded→degraded, unhealthy→needs_attention, unknown→unknown
(`room0.py:36`). An unrecognised tone reads as `update`, logged (`room0.py:157`). A card that
cannot be read honestly is `degraded` with every value `unknown` — never a made-up value. A card
past `stale_after_s` is `stale`, never `current` (`room0.py:434`).

**5. publish (selective)** is where this connection earns the adapt in §7.3: the owner chooses
which of the room's cards land on Home. Today every card lands (`room0.py:493`). Under §4.4 the
rest stay in Connect. Links remain same-origin paths or they are dropped (`room0.py:71`).

**6. propose-action → approve elsewhere → execute.** The whole point of `authority: "external"`:

```text
Connect: propose "Acknowledge the build plan"
   → ActionProposal { effect: "write", exposed: false, idempotency: "required",
                      approval: { kind: "project_home",
                                  room_url: "https://project-home.lan.example",
                                  action_ref: "<the room's own action ref>" } }
   → the owner is sent to Project Home; Worlds does not render an approve button here
Project Home: approves in its own /room/actions
Connect: endpoint 12 records the approval id
   → authority: project_home:<approval id>   (dispatcher.py:289)
Connect: execute → request_authorization → approve(recorded, not re-decided) → consume → dispatch
```

Worlds shows and links. It never mints a second approval for an operation Project Home already
governs, and it never approves one on the owner's behalf in Worlds. A room that offers its own
`ask_first` approval token is not supplied by Worlds: that action fails closed at the room
(`CONTRACTS.md` C8).

**Fast on a bad day is part of the contract, not an implementation detail.** A failing room is not
retried for 30 s, doubling to 5 min; a request waits at most 0.25 s behind another fetch; a room
always answers with last-good cards marked stale, or one explicit `unavailable` status card
(`room0.py:302`, `:470`, `:493`). One room being dead must never delay or blank the others — that
is a property of the contract, and it is what makes "Worlds shows" honest rather than fragile.

## 10. What this document does not decide

- Whether any of this is built. It is a MOCKUP. A PROTOTYPE would name the module and the tests;
  APPROVED would name the issue that accepted it.
- Whether the already-live `/api/connect/try` and `/preview` paths move. Default: they do not
  (§6). Moving a working path for tidiness is the same mistake as a new backend for newness.
- How endpoint 12 verifies a Project Home approval, given the unwired verifier (§7.3).
- The Connect UI design. "A brand new design" is Rylee's call and it is not this document's.
- Whether the 16 shipped recipes become descriptors, imports, or retired files. Endpoint 2 can
  carry them; what happens to the directories is a separate decision with its own evidence.