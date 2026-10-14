# Worlds full design: put the new art into the prototype

## Decisions in force
- 2026-10-13 Rylee: "I like this but would like new art generated for it and for the prose to be reduced down to just… terse warmth?"
- Protect the look, the feel, Dot, the warmth and the small delights. The copy has already been rewritten to terse warmth; do not change wording.

## Inputs
`docs/vnext/prototype/index.html` (the prototype, terse-warmth copy already in) and the new art in `docs/vnext/prototype/art/` (transparent WebP + PROVENANCE.md):
- Dot: `dot.webp` (neutral, awake), `dot-asleep.webp`, `dot-welcome.webp` (waving), `dot-nod.webp`, `dot-parcel.webp` (carrying the parcel), `dot-goodbye.webp`, `dot-curious.webp` (peering at an old slip: use for stale).
- Services: `icon-media-shelf`, `icon-front-door`, `icon-nightly-backup`, `icon-garden-camera`, `icon-studio-printer`, `icon-notes-sync`, `icon-local-weather` (map by fixture service id), and `icon-counter` (the app emblem, top bar).
- Scenes: `hero-welcome.webp` (welcome screen), `empty-counter.webp` (empty counter: Dot asleep by the counter), `stamp.webp` (the "Connected" stamp; keep the word "Connected" as real text beside or over it).

## Do
1. Replace the drawn inline-SVG Dot, stamp and welcome/empty illustrations with these images, each in the state the spec gives it (asleep when empty, welcome on the welcome, nod on a state change, parcel during publish, goodbye after a disconnect, curious beside stale). Keep the existing motion (nod, parcel walk, stamp landing) applied to the images, with the same reduced-motion forms.
2. Add the service icons to each service's slip, its surface header and the Connect discover list, always beside the service name (never alone), `alt=""` since the name is there; the counter emblem goes beside "Worlds" in the top bar.
3. The file must stay ONE self-contained HTML file: embed every image as a base64 `data:image/webp` URI (write a small script in your clone to do it; do not hand-type base64). Define each once (e.g. in a JS map or CSS custom properties) and reuse it.
4. Images are decorative; Dot's images get `alt=""` and `aria-hidden="true"` (the words carry meaning). Sizes: Dot about 72-96px tall in slips/status, up to 160px in the welcome and empty states; icons 28-40px; hero up to 360px wide on desktop and 220px on phone. They must look right in dark and light themes (they are transparent).
5. Do not change wording, ids, logic or layout beyond what the images need. Checker ids must keep working: `#skip-welcome`, `#add-service`, `#disconnect` (confirm button "Disconnect"), `#simulate-change`, `#live-toggle`.
6. Copy `docs/vnext/prototype/art/PROVENANCE.md` content unchanged; add one line in `docs/vnext/worlds-design.md` section 6 (Components) pointing to it as the art source.
Write only `docs/vnext/prototype/index.html` and that one line in the spec. Do not push.
