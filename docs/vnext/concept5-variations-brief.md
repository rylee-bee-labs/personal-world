# Worlds vNext — Concept 5 ("The Counter") flow variations, in the house design language

Status: MOCKUP brief. Nothing produced from it is approved.

## Decisions in force (quoted)
- 2026-10-13 Rylee: "Go with concept 5 from here. Bring it into our design language and have it make a few variations based on the overall flow please"
- 2026-10-13 Rylee, earlier: "the more real application with whimsy and warmth laced in but make sure they have both desktop and mobile and are reactive <3"

## Start here
- Concept 5 is `docs/vnext/concept5/DESIGN.md` and its prototype `docs/vnext/concept5/index.html` ("The Counter", with Dot the snail). Keep what makes it Concept 5: the counter and service slips, the fact's journey (Source → Worlds → Agent / You), the honest fact rows, Dot, the six-step Connect, the four destinations.
- Everything in `concept-brief-v2.md` and `concept-brief-v3.md` still applies (integration requirements 1-6, floors, real application, whimsy and warmth laced in, desktop 1440 AND mobile 390 designed on purpose, reactive live feed, checker ids `#add-service`, `#disconnect` visible on the first screen, `#simulate-change`, `#live-toggle`, MOCKUP label).

## Our design language (the house language)
The shared Rylee-Bee Design System, Mulberry & Mint base:
- `docs/vnext/house/DESIGN.md` (naming, roles, modes, pattern rules) and `docs/vnext/house/tokens.css` (the canonical token values). **Use these token names and values**; copy the `:root` and dark-mode blocks you need into your inline CSS. Do not invent colours, spacing or radii outside them.
- `docs/vnext/house/worlds-mulberry-mint.css`: how Worlds applies Mulberry & Mint today (dark by default, mint accent `#a7e6ce`, teal focus ring `#72b1b1`, Atkinson Hyperlegible body, Young Serif display). No external fonts: use those names first in the font stack with system fallbacks.
- `docs/vnext/house/LANGUAGE.md` (human words map onto Play-Nice state meanings: Needs you, Working, Held, Done, Failed, Unknown; never invent a state) and `docs/vnext/house/JOY-CURIOSITY-AND-CARE.md` (how joy and warmth are allowed to show up).
- Rylee's standing preferences: dark theme by default, soft contrast (no harsh pure white on black), reduced motion respected, full keyboard support, 44px targets.

## The variations: same Concept 5, different overall flow
Each variation is the whole app, but organised around a different way a person moves through it: empty → connect → composed home → embedded app → a change → disconnect. Your variation's flow is assigned below. Make the flow obvious within 10 seconds of opening it, and keep the other destinations reachable.
- **Counter-first (home-first):** opens on the composed counter; everything starts from the slips; Connect is a side action. The closest to Concept 5.
- **Welcome-led (onboarding-first):** a warm guided first run with Dot: empty counter → connect the first service → the second → the home reveal; afterwards it settles into the counter. Skippable.
- **Ask-the-counter (command/search-led):** one input ("connect notes", "open front door", "what needs me?") with suggestions drives navigation and actions; the counter is the result surface. Fully keyboard-first, still tappable on phone.
- **Today's slips (timeline/feed-led):** the day's events stream in as slips; tapping one opens the service, its facts and its embedded app; a new service arrives as a "new arrival" slip that starts Connect.

DESIGN.md (about one page) must describe the flow step by step for desktop and for mobile, and say what changed from Concept 5 and why.
