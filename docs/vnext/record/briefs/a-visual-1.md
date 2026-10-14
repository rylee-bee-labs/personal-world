# Worlds vNext — visual direction 1 of 3 (MOCKUP)

This brief is identical for all three directions except 1. Three different model families each get one direction, and the results are compared. Do your own best work; do not try to guess the others.

## What Worlds is
Worlds is a person's home for the independent services they run (media, files, home automation, a project dashboard, anything with an API). It shows what matters first, links out to the system that owns each thing, and still works with zero services and zero AI providers connected. It never invents a status: unknown and stale are said in words, not only colours. External systems keep their own authority; Worlds shows and links, it does not take over.

Rylee, 2026-10-13: "I'm wanting it to have a brand new design and new api connect layer." The current UI (FdApp, the Recipes/Connect tabs in ui/) is NOT a template. Make something structurally new: universal, inviting, modern and fun. Existing Worlds art or characters are optional inspiration only.

## Decisions in force (quote, do not paraphrase)
- 2026-10-13 Rylee: "I'm wanting it to have a brand new design and new api connect layer. Honestly I don't know what all we're doing with it haha. It follows our newer gated jobs."
- 2026-10-13 Rylee: "Not yet. Wanna get it cleaned up first then we design." then "Ok do add the api and visuals. Hah" — visuals are in scope; cleanup runs first.
- 2026-10-13 issue #321/#322: a mockup is not an approval. Status words MOCKUP != PROTOTYPE != APPROVED; Rylee picks the direction.
- These are decisions, not suggestions. Never change code, tests or docs to contradict one, even to satisfy a review finding. If your task seems to need that, stop and say which decision is in the way.

## Your direction
Direction 1 must have its own clear idea, named in one line at the top of your README (for example a spatial metaphor, a feed, a calm dashboard, a desk, a map). Structure, layout and interaction must differ, not just colour.

## Deliverables (in your clone, under docs/vnext/visuals/direction-1/)
- README.md: the one-line idea, who it suits, 3 strengths, 3 risks, and what a later Connect API must supply for it.
- Self-contained HTML (inline CSS, no external network, no build step), for each of the four states:
  1. quiet day (several services, all fine),
  2. needs-you (one service wants a human decision, linking out to the owning system),
  3. stale / unknown (one source stale, one unknown, both labelled in words),
  4. no services (first run, zero providers, still useful and inviting).
  Each state at 390px phone width AND desktop width (one responsive page per state is fine if both widths are shown and correct).
- Every page shows a visible "MOCKUP" label.
- Use realistic fake content (service names like "Media library", "Backups", "Home lights"); no real hostnames, tokens or personal data.

## Floors (fail any and the direction is rejected)
- Text contrast WCAG AA, 44px touch targets on phone, visible keyboard focus, semantic headings, status never shown by colour alone, reduced-motion respected, no horizontal scroll at 390px.
- No fabricated provider results presented as real.

## Rules
- Work only in your clone. Do not push, deploy, or touch src/ or ui/.
- When done, list every file you created and the one-line idea.
