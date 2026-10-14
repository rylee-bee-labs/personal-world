# Worlds design v3: finish the last 3 checklist items (small targeted edit)

## Decisions in force
- 2026-10-14 Rylee tapped "Finish the 3 items" on the budget card: one small fix pass. Prefer small targeted edits over a rebuild.
- Copy stays terse warmth (spec "Voice: terse warmth"). Protect the look and Dot.

Files: `docs/vnext/prototype/index.html` (art is referenced as `art/<name>.webp` in one ART map; leave that map alone), `docs/vnext/worlds-design.md` (checklist items RX-01, RB-23, RB-24 are the only unticked ones).

1. **RX-01**: slice rendering on the existing store: the live tick must repaint only age text and freshness badges (not whole views); state changes repaint only the views subscribed to the changed slice. Keep the store API; don't restructure the app.
2. **RB-23**: slips get an `aria-label` written once at render (name, state word, and "selected" when selected); the ticking age span is `aria-hidden="true"`; never announce ticks; announce only state-word changes, selection ("Selected Front Door."), connect, disconnect, undo, and errors, all through the ONE existing polite live region, in terse warmth.
3. **RB-24**: contrast was measured by Claude with axe across all four themes (dark, light, high-contrast, low-stimulation) on welcome, home, connect and needs-you: zero violations. Under `forced-colors: active` axe flags authored colours (mockup tag, wordmark, accents); add a `@media (forced-colors: active)` block that maps text, borders, focus rings, badges and status chips to system colours (CanvasText, Canvas, LinkText, Highlight, HighlightText, GrayText, ButtonText, ButtonFace), keeps status words visible, and keeps the art visible (`forced-color-adjust: none` on images only).
4. In the spec: tick RX-01, RB-23 and RB-24 for the automated part, and add a line under RB-24: "Human screen-reader and contrast check: NOT RUN, owner check (Rylee)."
Keep the checker ids (`#skip-welcome`, `#add-service`, `#disconnect` with confirm "Disconnect", `#simulate-change`, `#live-toggle`, `#undo`). Write only those two files. Do not push.
