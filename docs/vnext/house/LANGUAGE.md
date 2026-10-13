# Shared language boundary

The design system does **not** own a second machine vocabulary.

## Machine/shared meaning

Play-Nice owns shared machine semantics, including the closed status vocabulary:

```text
healthy warning needs_attention degraded unavailable
not_configured disabled stale unknown

working waiting blocked deferred partial complete failed
```

If a product/provider has other state words, translate them at its boundary.

## Human presentation

This repo may define short human labels and their visual treatment when they
preserve the underlying Play-Nice meaning.

Current examples:

| Human label | Play-Nice machine meaning |
| --- | --- |
| Needs you | `needs_attention` when a person actually must act |
| Working | `working` |
| Held | `deferred` when intentionally parked; the reason must remain inspectable |
| Done | `complete` |
| Failed | `failed` |
| Unknown | `unknown` |

These are presentation mappings, not aliases for every possible situation.

For example, `blocked` does not automatically become **Needs you**. It may be
blocked on a machine or another participant. Preserve the distinction.

## Product words

Products own their own domain/product language where Play-Nice deliberately
does not.

The design system may show how such words look. It does not seize ownership of
their meaning.

## Character and voice

Plain human wording comes from machine truth.

Character/theme voice may add warmth after that translation, but cannot create
state, authorization, urgency or certainty.

```text
machine truth
→ plain human wording
→ optional product/character voice
```

Nothing downstream is parsed back into machine state.

## Research language

Do not create a closed local status enum for research maturity.

Record:

- source/provenance,
- reviewed: true/false,
- what was observed,
- whether an explicit decision adopted it,
- historical/superseded state where appropriate,
- UNKNOWN when evidence is missing.

The candidate Estate Words / Book Grammar experiment is a useful research
source, not an authority until explicitly promoted.

## Test

A new design-system word should answer:

> Is this a new meaning, or merely a clearer human rendering of meaning we
> already have?

If it is shared machine meaning, it belongs in Play-Nice, not here.
