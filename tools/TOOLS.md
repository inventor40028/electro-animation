# ELECTRO ANIMATION — Tools

Runnable pipeline tools copied from the two kits (new `cartoon-short-kit` + old
`claude-animated-shorts`). **Any AI or human can drive a full short from here.**

## Prerequisites
- Node 22.6+, Python3 (numpy + Pillow), ffmpeg/ffprobe, Playwright Chromium
  (headless SwiftShader flags: `--use-angle=swiftshader --enable-unsafe-swiftshader`).
- The engine expects `web/` with `index.html`, `main.js`, `story.js`, `motion.js`,
  `paint.js`, `roman.js`, `toon.js` (= `toonbase.js` + `roman.js`), `lib/three.module.js`.

## The tools

| Tool | What it does | Usage |
|---|---|---|
| `render.mjs` | Render frames from stills/full range | `node render.mjs <outdir> "<a-b or 1,2,3>" [step] [quality]` — env `WEB=.../web`, `HASH`, `RESUME=1` (skip existing). |
| `setup.sh` | Scaffold a new project (web/ + lib + three.js + toonbase) | `bash tools/setup.sh <project_dir>` |
| `timing.py` | Word/phrase timing from voice + script | `python3 timing.py voice.mp3 [script.txt] [--out work/]` → `words.json`, `phrases.json` |
| `sheet.py` | Contact sheet from stills | `python3 sheet.py stills/x sheet.jpg [cols=7]` |
| `finish.sh` | Loudness-normalize + encode final MP4 + QA | `bash finish.sh "Title"` (uses `mix_raw.wav`) |
| `short_qa.py` | Frame-by-frame QA (loudness, silences, frozen/black/pop, per-shot colour) | `python3 short_qa.py ...` |
| `bridge_merge.py` | Merge ownership-scoped sandboxes | `python3 bridge_merge.py` |
| `bridge_shots.py` | Write `shots.json` from story.ts | `python3 bridge_shots.py` |
| `bridge-stills.mjs` | Render stills (GPU/ANGLE) | `node bridge-stills.mjs ...` |
| `sbs_sheet.py` | Side-by-side review sheet | `python3 sbs_sheet.py ...` |

## Audio suite (`examples/reference-audio/`) — from the old kit
Full synthesized toolkit (numpy/scipy, no third-party audio):
- `dsp.py` — filters, modal, reverb IR, BS.1770 loudness, WAV IO.
- `instruments.py` — brass, glock, vibes, musicbox, triangle, fife, whistle, snare,
  cymbal, timpani, brush, pluck, pad.
- `music.py` — composition (progressions, sections, orchestration, reverb sends).
- `sfx_gen.py` — stick-slip creaks, boots, stitching, splash, birds, cor formal etc.
- `sfx.py` — event cue class + ambience beds + distance-to-mix rule.
- `timeline.py` — mirrors the shot plan; computes distance FX per sound.
- `mix.py` — build + duck + look-ahead limiter → `mix_raw.wav` + stems + `cues.md`.

## The new-kit engine (`engine_reference/`)
`main.js`, `motion.js`, `paint.js`, `roman.js`, `index.html` — the proven renderer
(ink/depth/DOF/grade/lights/cards). Use as the base for any new `web/`.

## Example (complete, runnable)
- `examples/example-story.js` — full shot plan for "The Soldier Who Never Left"
  (the new kit's worked example).
- `examples/example-audio.py` — its music/SFX mix.
- `examples/story-plan.md` — our Story-1 story.js patterns.

## Full pipeline (quick reference)
1. `bash tools/setup.sh ~/shorts/my-short`
2. `python3 timing.py voice.mp3 script.txt --out work`
3. Write `story.js` (shots/cameras/lights/actors/fx per ELECTRO_ANIMATION.md).
4. `node tools/render.mjs stills/a "5,40,..."` → `python3 tools/sheet.py stills/a sheet.jpg`
   (send sheet to user).
5. `node tools/render.mjs out/frames "0-591"` (or `RESUME=1` to continue).
6. Audio: use `examples/reference-audio/mix.py` (or `audio_xqivo.py` style) → `mix_raw.wav`.
7. `bash tools/finish.sh "Title"` → final MP4 + QA sheet.
