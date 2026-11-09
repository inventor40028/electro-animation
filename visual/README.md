# ELECTRO ANIMATION — Visual Gallery

Curated **actual frames** and contact sheets for a visual AI (or human) to learn the target
look from. These are the real outputs of the reference pipeline — not descriptions.

## Contents

### `frames/` — 38 key-frame stills (540×960)
Extracted frames from the 20 reference shorts (2 per short: `shortNN_0060` and
`shortNN_0180`). These are the **style ground-truth**: character proportions, ink,
lighting, grade, camera framing, environment detail.
- Source: `claude-animated-shorts/references/frames/`.

### `sheets/` — 20 contact sheets (450×480 previews)
One per reference short; each is 2 s of footage tiled into a sheet. Great for quickly
reading a short's overall grade/mood/camera across time.
- Downscaled from 1800×1920 originals to keep the gallery compact.
- Source: `claude-animated-shorts/references/sheets/`.

### `soldier_example_stills.jpg` — the new-kit worked example
A contact sheet of "The Soldier Who Never Left" (the new kit's completed example). This is
the **runnable engine's actual output** — the closest match to what a new short built in
`electro-animation` will look like.
- Source: `cartoon-short-kit/example/soldier/stills.jpg`.

## How a visual AI should use these
1. Skim `sheets/` to lock the overall look per mood (day / golden / night).
2. Zoom into `frames/` for exact character/ink/lighting details.
3. Use `soldier_example_stills.jpg` as the "what the engine produces" reference.
4. Cross-reference the numbers in `reference/` (GRADE, CHARACTERS, CAMERA, WORLD_INK,
   ENVIRONMENTS) against what you see here.

> Note: these are reference material for matching style only. Never trace or copy the
> specific characters/props/shots — general rules and numbers only (per the old kit's
> own study constraints).
