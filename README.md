# ELECTRO ANIMATION

A unified animation-authoring system for Xqivo shorts. Merges the **new**
`cartoon-short-kit` (engine, tools, example) and the **old** `claude-animated-shorts`
(style bible, measured study reports) into one reference brain, plus the finished
`shorts-xqivo` "5 PM Time Trap" production as a proven working example.

## Start here
- **`ELECTRO_ANIMATION.md`** — the master system: the 8 Agents, merged defaults,
  production pipeline, reference index. **Read this first, every time.**
- `reference/` — per-agent rule docs:
  - `CAMERA.md` (Agent 2) — glide grammar, framing, DOF, kicks.
  - `CHARACTERS.md` (Agents 1 & 3) — head/body numbers, expressions, reaction rules.
  - `ENVIRONMENTS.md` (Agents 4 & 6) — environment fill, props, lighting, action logic.
  - `WORLD_BACKDROP.md` (Agent 8) — build the world beyond the set; keep the grey off.
  - `AUDIO_MIX.md` (Agent 5) — SFX mapping, level contract, cues.
- `examples/` — working code patterns from the finished short (shot plan, camera, DOF,
  actors, FX), plus a complete runnable example (`example-story.js`, `example-audio.py`)
  and the full old-kit audio synthesis suite in `examples/reference-audio/`.
- `tools/` — runnable pipeline tools (render, setup, timing, sheet, finish, QA, merge).
  See `tools/TOOLS.md` for usage. `engine_reference/` holds the base renderer.
- `implementation/bridge/` — the complete canonical implementation (13 files: story,
  world, build, toon, cast, lights, shade, cardshade, ink, lensblur, grade, Bridge.tsx).
  See `implementation/IMPLEMENTATION.md`.
- `visual/` — actual frames & contact sheets for a visual AI: 38 key-frame stills
  (`visual/frames/`), 20 downscaled contact sheets (`visual/sheets/`), and the soldier
  example stills. See `visual/README.md`.**

## The 8 Agents (summary)
1. **Reaction Control** — expressions match the beat (calm=calm, shock=shock).
2. **Frame Inspection** — camera glides, no bad crops/zoom, subjects visible.
3. **Animation QC** — result matches the prompt; flag & fix anything off.
4. **Environment Research & Design** — fill underspecified environments realistically.
5. **Environmental Events & Sound** — show events AND give them sound.
6. **Scene Context & Action** — every action has a cause, reaction, consequence.
7. **Final Consistency** — all of the above coordinated; a coherent film, not stills.
8. **World/Backdrop Builder** — build the world beyond the set; keep the grey off (wide
   shots never show a flat grey void). See `reference/WORLD_BACKDROP.md`.

## Proven pipeline
Script → Voice Studio (ElevenLabs-compatible local server) → `story.js` shot plan →
`node render.mjs` frames → verify with stills (Agents 1–3) → `audio_xqivo.py` mix →
ffmpeg mux → final MP4.

See `ELECTRO_ANIMATION.md` §3 for the exact commands and the local TTS API.
