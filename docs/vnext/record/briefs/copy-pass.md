# Worlds full design: "terse warmth" copy pass + one bug fix

## Decisions in force
- 2026-10-13 Rylee: "I like this but would like new art generated for it and for the prose to be reduced down to just… terse warmth?"
- 2026-10-13 Rylee: "comprehensive but also keep the original feeling." Clarified: protect the feel, the look, the whimsy and the love, NOT the copy.
- Plain words: `docs/vnext/house/LANGUAGE.md` (state words map to Play-Nice meanings; never invent a state).

## Do
1. In `docs/vnext/prototype/index.html`, rewrite EVERY visible string (headings, body text, buttons, labels, hints, toasts, dialogs, empty states, errors, aria-labels and live-region announcements) to **terse warmth**: as few words as possible while staying kind. Short sentences or fragments, warm not cute, no exclamation marks, no filler ("Worlds will…", "This means that…"), keep the meaning and every honest fact (source, freshness, who can see, who can act, what is kept, what goes, fail-closed approvals, proposals can't run). Examples of the target:
   - "Nothing at the counter yet. Connect a service and Worlds will keep its facts, say where each came from, and leave the deciding to you." → "Quiet here. Connect something?"
   - "Media Shelf's 3 facts and its app frame are removed from Worlds. The Media app is untouched. Its reference is kept." → "Media Shelf's gone from here. The app's fine. Reference kept."
   - "Passed. Worlds read 3 facts in 257ms (simulated). Nothing on Media Shelf was changed." → "Passed. 3 facts read. Nothing changed."
   Keep state words from LANGUAGE.md exactly (Healthy, Needs you, Stale, Unavailable, Unknown, Not configured…). Keep the MOCKUP label.
2. Fix the bug: fact rows say "observed Never checked" while marked Live. A row must never claim Live without a real checked time; show the checked time from the store (e.g. "checked 12s ago") and keep it ticking with the live feed; if a fact truly was never checked, its state word must not be Live.
3. Do NOT change element ids, classes used by JS, logic, layout or styles, except where a string change needs it. The checker ids must keep working: `#skip-welcome`, `#add-service`, `#disconnect` (confirm button labelled exactly "Disconnect"), `#simulate-change`, `#live-toggle`.
4. In `docs/vnext/worlds-design.md`: add a short section **"Voice: terse warmth"** right after "The feeling to protect" (the rule in 3-5 lines plus 5 before/after pairs), and rewrite the spec's own UI-copy sections (section 7 Copy, and any quoted UI strings in the screen tables) to match the new strings. Do not rewrite the rest of the spec.
Write only those two files. Do not push.
