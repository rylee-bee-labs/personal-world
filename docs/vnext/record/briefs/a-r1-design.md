# Worlds vNext — round 1b: a mini DESIGN.md for each of your concepts

## Decisions in force
- 2026-10-13 Rylee, after seeing the 12 round-1 concepts: "they seem to not quite get the fact it needs to be integrated, they seem like concepts on a page. maybe have them develop a mini design.md for each build? I don't hate them they're just not quite.... ready for a bigger test"
- Nothing here is approved. Concepts stay MOCKUP.

## Your job
You made three concept prototypes in round 1. Their folders are in this clone: {SLUGS} (each under `docs/vnext/round1/<slug>/`, with `index.html` and `README.md`). Other folders in `docs/vnext/round1/` belong to other authors: do not read them for ideas and do not touch them.

For EACH of your three concepts, write `docs/vnext/round1/<slug>/DESIGN.md` (about 1-2 pages). It turns the page-concept into a design for a **real, integrated product**. Rylee's complaint is that the concepts look like a single page; the DESIGN.md must show how the concept works as a whole app that real services plug into.

## What Worlds really is (the integration it must fit)
- Worlds is the human-facing home for independent services a person runs. Services keep their own data and authority; Worlds shows facts, links out, and embeds supported apps.
- Project Home owns operational authority (approvals, missions). Companion owns chat. Worlds never approves or executes on its own; a governed action is a link/handoff to the owning system.
- A new provider-neutral Connect API (in design, `docs/vnext/connect-contract.md` on another branch) gives Worlds: discover a service, register it with a **credential reference** (never a secret in the browser), a safe test, a preview of what facts/capabilities it offers, publish chosen facts to Home, a governed action link, and disconnect/revoke.
- Local Memory, accessibility, zero-provider usability (Worlds is useful with no AI and no services) and honest status (stale / unknown said in words) are protected.

## DESIGN.md sections (use these headings)
1. **The idea in one paragraph** and the main gesture.
2. **App map**: every screen or mode beyond the home view, at least: home, add/connect a service, service detail, an embedded app, shared facts (agent vs human), settings/Memory, and how you move between them on phone and desktop.
3. **Integration walkthrough**: connect ONE generic HTTP service end to end in this concept: discover → credential reference → test → choose facts → it appears on home → it goes stale → a governed action hands off to the owning system → disconnect. Say what the user sees at each step.
4. **Data it needs**: which Connect API facts/capabilities each part of the UI consumes (name them), including provenance and freshness. No field without a consumer.
5. **Embedded apps**: what can embed, how it is sandboxed, what happens when embedding is refused, and how the embedded app shares facts with the shell.
6. **Agents and people**: what an agent can see and do vs the human; how permissions and provenance are shown.
7. **States**: zero services/zero providers (first run), quiet, needs-you, stale, unknown, revoked, offline/error. One line each on how the concept shows it.
8. **Scale and growth**: what happens with 3 services, with 30, and when a new kind of service arrives.
9. **Accessibility**: keyboard path through the main gesture, screen-reader structure, reduced motion, phone targets.
10. **What changes in the prototype next**: the 3 most important changes to make the round-1 page feel integrated (do not make them now).
11. **Risks and open questions.**

## Rules
- Write only the three DESIGN.md files. Do not change any index.html or README.md. Do not touch src/ or ui/. Do not push.
- Be concrete and honest; say UNKNOWN when you don't know. When done, list the three files.
