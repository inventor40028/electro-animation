# ELECTRO ANIMATION — Master System

The unified animation-authoring system for Xqivo shorts. Merges the **new** kit
(`cartoon-short-kit/`) and the **old** look-study kit (`claude-animated-shorts/`)
into one reference brain. **Read this file before any animation work; then pull the
specific rule docs under `reference/` when a given agent is active.**

---

## 0. How this works

- **Every animation task runs the 7 Agents below.** They are not optional phases — they
  are roles the lead/Claude must mentally run before, during and after authoring.
- Concrete, measured rules come from the study docs (not vibes). Numbers in this file are
  the merged, trusted defaults.
- The old kit's *studies* told us WHAT looks right (camera glide, face shape, ink weight,
  lighting). The new kit's *engine* tells us HOW it is rendered (story.js shots,
  render.mjs, audio_xqivo.py). Use both.

---

## 1. The 7 Agents

### Agent 1 — Reaction Control
Inspect character reactions/expressions after authoring and confirm they match the story.
- Shocking/surprising beats → big, readable reaction (eyes, mouth, body settle) landing
  **0–3 frames before** the spoken word/event.
- Normal beats → normal face + body. No exaggerated faces for everyday lines.
- Every reaction must be justifiable from context. If a line is calm, a huge gasp is wrong.

### Agent 2 — Frame Inspection
Continuously inspect rendered frames for camera correctness.
- No excessive zoom, unnecessary movement, poor positioning, awkward crop, or unwanted
  angle changes.
- Characters/objects must remain visible; the camera must behave naturally.
- Concrete frame-pass (see `reference/CAMERA.md`): glides, envelope drift, headroom,
  no dead-still, no over-long kicks.

### Agent 3 — Animation Quality Control
Verify the render matches the user's request/prompt.
- Compare against the original prompt: characters, environment, actions, objects, events.
- Flag anything missing/incorrect/different; ensure it is corrected before delivery.
- Keep a checklist per request: structure → setting → characters → action beats → payoff.
- Check every segment's continuity with the previous one (no unexplained cuts).

### Agent 4 — Environment Research & Design
When a user gives an environment without full detail, research and fill it realistically.
- Example: "office" → desk, chair, computer/monitor, keyboard, mouse, window, lighting,
  plant, wall clock, door, poster, paper stack, mug, lamp, ceiling fan.
- Example: "forest" → trees, undergrowth, light shafts, fallen branches, birds, sounds.
- Produce detailed environment descriptions for the scene. See `reference/ENVIRONMENTS.md`.

### Agent 5 — Environmental Events & Sound Effects
Identify natural events in an environment; show them AND give them sound.
- Coffee spills → liquid spreads, reacts with objects, plus a sound.
- Tree falls → falls, branches break, reactions, plus sound.
- Always consider environment + event before deciding visuals/sound. See `reference/AUDIO_MIX.md`.

### Agent 6 — Scene Context & Action
Understand the storyline; decide what each beat shows and why.
- Know what is happening, why, and what comes next.
- Every action needs a natural reaction + consequence (spill → annoyance → clean-up, etc).
- Tie physical interactions to consequences so the viewer follows the cause/effect.

### Agent 7 — Final Consistency
All agents work together for a consistent, realistic, faithful result.
- Every environment detailed, every action has a consequence, every character reacts
  appropriately, every sound matches the visuals.
- Goal: a coordinated sequence of events/actions/reactions/environment/camera/sound —
  not just moving images.

### Agent 8 — World/Backdrop Builder (keep the grey off)
A dedicated role, separate from Agent 4 (which fills props INSIDE a room). This agent owns
everything the camera can see OUTSIDE the set. Its job: no wide shot / pull-back / zoom-out
ever shows a flat grey void.
- Read the shot plan; find every shot where the camera sees beyond the set.
- Build the backdrop world: ground continuation, hills, skyline/town/forest silhouettes,
  mountains, sky gradient; tint distant layers toward the fog/horizon colour.
- Ensure openings (windows/doors/open sides, or a purpose-built back-wall window) reveal it.
- Rules + recipe + QC checklist: see `reference/WORLD_BACKDROP.md`.

---

## 2. Merged defaults (trusted numbers)

### Camera (from old motion + depth studies; new kit renders it via `CAMS` + `camAt`)
- **Glide, don't snap.** Moves are symmetric ease-in-out (~10 f in, ~10 f out, ~23 f
  total). Peak speed ~14–16 px/f (at 720p ≈ ×1.5 for 1080p). Nobody moves 3-frame snaps.
- **Almost never dead still.** ~65% of frames move. Quiet frames keep a slow push-in
  (~0.14%/f median ≈ 0.0015–0.0035/f; ~0.6%/f for ~35 f on punch words). No hand-held
  float (delete sine float).
- **Headroom:** head top at y ≈ 0.28 (CU) / 0.35 (MS) / 0.38 (FS) / 0.50 (WS). Horizon ≈ 0.45.
- **Blinks:** every 0.5–2.3 s, 4 frames (half/shut/shut/half) + one on every expression swap.
- **Kicks/impacts:** 6–7 frame kicks, sign flips every frame, ~3 px offset, roll ±0.23°,
  gone in 6–8 f. Whip pans (45–93 px/f over 7–25 f) only with strong motion blur.
- **Camera height:** ~0.79 × character height; mostly eye level; avoid too-low shots.
- **DOF:** background DOF on ~⅓ of shots (sigma720 ≈ 2–4). Foreground blur rare but strong.
  No rack focus inside a static shot. Bloom → ink → DOF → tone (bloom BEFORE ink or it
  thins lines 22–28%).

### Character (from old study; new kit draws via `roman.js` cast)
- Heads: H/W ≈ 0.93 (rounder than ours were), egg taper ≈ −0.125, crown n_top ≈ 1.7,
  jaw n_bot ≈ 2.26, asym ≈ +0.10, slight tilt, subtle wobble. Eyes/mouth shift toward the
  side the character faces. No catchlights blobs.
- Bodies: adults ≈ 3.5 head-heights; bean torso; thin ink-stroke legs; ink-dot hands.
- Outline: 5% W median, thick/thin ~2.9–3.5 (heavy on bottom/shadow side).
- Front vs side: `faceFront` (full face) and `faceSide` (profile, one eye + nose line).
  Side view = profile head + side body.

### Lighting / grade (from old lighting study)
- Day shadows: Y ratio shade/lit ≈ 0.49–0.52 (not 0.25). Keep colour, don't go grey.
- Golden/sunset: backlight (sun ahead, az 0–40°, el 3–15°), long violet-blue shadows.
- Sky day: upper #5e93d9, horizon #9fb6e6. Golden: upper mauve #9a7e7f, horizon #ffd18a.
- Heads: bright (V ~0.97), side gradient toward key, slight top brightening, warm rim.
- Bloom: only for emissive sources (moon/lamp/fire/sun), e-fold ~14–21 px @720p.
- Ink/outline: ~3 px @1080p, near-black, on ~74% of geometry edges (silhouettes, depth
  jumps, creases, contacts). Crisp, then blurred by DOF along with background. Ink colour
  #0b0a0c; fades 150–450 m; blend in display gamma; 3×3 tent AA.
- Grade targets (day): Y' p50 ≈ 0.57, p99 ≈ 0.97, non-ink p5 ≈ 0.23 (no crushed blacks),
  lit chroma C* p90 ≈ 55, vignette corner/centre ≈ 0.86, warm black point (35,19,20).
- Golden grade: violet/blue shadows (b* −6 not brown), ~25% violet/rose hues.
- Two-tone light model: `sunTerm = sunVis·smoothstep(0,uTerm,ndl)`, lit/shade colours,
  cool sky fill, warm rim — implement via `shade.ts`-style two-tone (see reference).

### Audio / mix (from new kit + our production)
- Voice is the master; music & SFX sit at **50% of voice RMS** (never louder).
- SFX must match visuals (spill, ding, slam, whoosh). No sound without a seen source.
- Target: 3.7–4.0 words/s narration. Beats land 0–3 f before their word.
- Export narration as WAV; mix via `audio_xqivo.py` (music+SFX+) final mux with ffmpeg.

---

## 3. Production pipeline (new kit end-to-end)

**Runnable tools live in `tools/` (see `tools/TOOLS.md`).** The full audio synth suite is
in `examples/reference-audio/`. The engine base is in `engine_reference/`. A complete
runnable example is `examples/example-story.js` + `examples/example-audio.py`.

**The complete canonical implementation is in `implementation/bridge/`** (13 TypeScript
files: story, world, build, toon, cast, lights, shade, cardshade, ink, lensblur, grade,
Bridge.tsx). See `implementation/IMPLEMENTATION.md` for what each file is and how it maps
to the reference docs. Use it as the authoritative quality bar.

**Visual examples live in `visual/`** — 38 key-frame stills (reference frames), 20
downscaled contact sheets, and the soldier example stills. A visual AI should read these
first to lock the look, then use the numbers in `reference/` to reproduce it.

1. **Script** → SCRIPT.md (hook → setting → escalation → snap → payoff → tagline).
2. **Voice** → Voice Studio (ElevenLabs-compatible local server, `http://127.0.0.1:3900/v1`).
   - `POST /v1/audio/speech` with `{model:"tts-1", voice:"<voice_id>", input, response_format:"wav"}`.
   - Custom voices live under `/v1/audio/voices` (e.g. `4024f5c2` = Electro, `ddd37b76` = brian me).
3. **Shot plan** → `story.js`: `SHOTS`, `CAMS` (keys glide), `LIGHTS`, `actorsAt`, `fxAt`.
4. **Render** → `node render.mjs out/frames "<a-b>" 1 0.95` (1080×1920, 30 fps).
5. **Verify** → stills via `node render.mjs stills/office "<frames>"`; run Agents 1–3 on stills.
6. **Audio** → `audio_xqivo.py` builds music+SFX, levels to 50% voice, writes `mix_raw.wav`.
7. **Finish** → ffmpeg mux frames+audio → final MP4.

---

## 4. Reference index (read the specific doc when its agent is active)

| Agent | Doc |
|---|---|
| 1 Reaction | `reference/CHARACTERS.md` (expressions, faces, poses) |
| 2 Frame | `reference/CAMERA.md` (glide, headroom, DOF, kicks) |
| 3 QC | `reference/CHARACTERS.md` + `reference/ENVIRONMENTS.md` (checklist) |
| 4 Environment | `reference/ENVIRONMENTS.md` (rooms, city, forest + object libraries) |
| 5 Events/Sound | `reference/AUDIO_MIX.md` (SFX mapping) |
| 6 Context/Action | `reference/ENVIRONMENTS.md` (cause→effect) |
| 7 Consistency | this file + spot-check all of the above |
| 8 World/Backdrop | `reference/WORLD_BACKDROP.md` (keep the grey off; world plotting) |

Heavy numeric references (read for the look):
- `reference/GRADE.md` — tone curve, per-zone saturation, three-way tint, vignette, material
  albedo targets.
- `reference/WORLD_INK.md` — the world outline pass recipe and composer order.
- `reference/ENVIRONMENTS.md` — texture/texel densities, sky stops, water shading.

Examples of real working code live in `examples/` (a shot plan, a camera block, an FX
helper, an audio cue add).
