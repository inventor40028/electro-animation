# CAMERA — Frame Inspection & Camera Grammar (Agent 2)

Source: merged from `claude-animated-shorts/references/reports/motion.md` + `depth.md`
and the new kit's `engine/motion.js` (`camFromKeys`). Use for every shot.

## Shot / angle vocabulary (vary angles — never one angle all film)
Use a MIX of shot sizes AND camera heights/positions. A film that stays at one angle
reads flat. Reference distribution: ~82% eye level, ~11% high, ~3% low, ~3% top-down;
shot sizes CU/MS/FS/WS/inserts all appear.

### Shot sizes (headroom → head-top y)
- **CU (close-up):** head-top y ≈ 0.28. For faces/symbols. Strong DOF (cinf high).
- **MS (medium):** head-top y ≈ 0.35. The workhorse dialogue/action shot.
- **FS (full):** head-top y ≈ 0.38. Shows the character + setting.
- **WS (wide):** head-top y ≈ 0.50. Establishing, scale, pull-backs. Background DOF low.
- **Insert:** close on an object (envelope, jar, card). Head-top rule N/A; use DOF to
  isolate (cinf moderate).
- **Over-the-shoulder (OTS):** a foreground shoulder/head frames the subject — adds depth
  and "we are watching" intimacy (reference: MS OTS used ~0.36 subject sigma).

### Camera height / angle
- Eye level (0.79 × character height) — default.
- **High angle** (above eye) — makes a subject feel smaller/vulnerable/diminished
  (use for the "quiet cost" beats).
- **Low angle** (below eye) — makes a subject feel larger/hopeful/commanding.
- **Top-down** — rare, for a graphic insert (jar, envelope, desk).
- **Profile/side** — character turned in profile (faceSide) for a side-on beat.
- **3/4** — between front and profile; the most common "turn" framing.
- **Dutch/roll** — only ~2% of shots; use sparingly for unease.

### Camera moves (grammar)
- **Glide** (smootherstep) — the default move; symmetric in/out (see below).
- **Push-in** — slow zoom toward subject (use on emotional beats; 0.15–0.35%/f).
- **Pull-back** (crane-out) — reveal/widen (establishing, endings).
- **Orbit** — camera circles a subject (use `path` spline; great for a hero object).
- **Pivot/pan** — camera rotates in place to follow action.
- **Cut** — hard cut for big reframes (never a huge glide).
- **Kick** — 7-frame flip-shake on impacts only.
- **Spline path** — `b.path = [[x,y,z],...]` gives a curved, corner-free move
  (for orbits and flowing reveals).

### Move pacing rules
- Peak speed ≤ ~15 px/f @720p (21–24 @1080p); a move needing more becomes a CUT.
- Move duration ~18–40f; attack=release (symmetric).
- Holds push in (creep 0.0012). No hand-held float.
- Kicks: 6–7f, sign flips every frame, ~3px, gone in 6–8f.
- Roll moves: ~3/10s, ~3.6° over ~18f.

### Exchange (every scene should vary what the camera does)
Pick a different SHOT SIZE and/or ANGLE for each new beat. Avoid two consecutive shots
with identical framing. The camera should feel like a storyteller moving to where the
meaning is, not a fixed tripod.

## Item-focus beat (when a prop is spoken about / becomes the subject)
When narration or action refers to a SPECIFIC object (a phone on the table, a cup on a
shelf, an envelope in a drawer), the camera should acknowledge it — it's a named subject.
The beat: **swing to the item → frame it as an insert → swing/pull back to the main
subject.** This is a stylised "spoken word lands on the thing" moment.

### Recipe
1. **On the word/frame** the item is named (0–3 f before), the camera begins a short
   glide (~12–24 f) toward the item — use a spline `path` for a smooth swing, not a cut.
2. **Hold the insert** for ~12–20 f with the item framed (head-top rule N/A; use DOF
   `cinf` moderate so it pops; optionally a slight push-in during the hold).
3. **Return** — glide back to the main subject (the character), usually to the same
   framing you left, over ~12–24 f.

### Rules
- Only trigger for **named/meaningful** items — not every prop. If it's mentioned, show it.
- The return should land on the character (the "main subject").
- Keep the move speeds within the glide budget (≤15 px/f @720p); use a spline for the
  swing so it arcs naturally.
- The item's insert should be composed clearly (center, shallow DOF), not a sloppy clip.
- If the item is off-camera, pan/swing to it; don't cut to a blank wall.

### Implementation (in story.js)
Add a dedicated shot id for the insert (e.g. `ins_phone`, `ins_cup`) or bake the swing
into the surrounding shot's keyframe list:
```
{ f: T0, pos: [...main framing...], look: [...main...] },
{ f: T0+12, pos: [...swing to item...], look: [...item...], path: [[...via...]] },
{ f: T0+28, pos: [...item insert...], look: [...item...], fov: 36 },
{ f: T0+48, pos: [...swing back...], look: [...main...] },
{ f: T0+70, pos: [...main restored...], look: [...main...] },
```
Give the item a `cinf` bump in `dofAt` so it reads as intentionally focused.

## Core rule: glide, don't snap
- Camera moves are **symmetric ease-in-out** (smootherstep): ~10 f in, ~10 f out,
  ~23 f total. Peak speed ~14–16 px/f at 720p (×1.5 for 1080p = ~21–24 px/f).
- **Never** a 3-frame snap. A move that would exceed ~25 px/f must be spread over
  more frames (or turned into a hard CUT).
- This is implemented by `camFromKeys` easing between `CAMS` keys with `smooth`.

## The camera is almost never dead
- ~65% of frames move. Even quiet frames push in slowly (~0.14%/f median).
- Use the built-in **creep** (`{creep: 0.0012}`) so holds never freeze.
- Delete any hand-held sine float — reference has none.

## Framing defaults
- Camera height ≈ 0.79 × character height; prefer eye level (82% of ref shots).
- Headroom — head top at y ≈: 0.28 (CU), 0.35 (MS), 0.38 (FS), 0.50 (WS).
- Horizon ≈ 0.45 of frame height.
- FOV ≈ 34–46° (median 40). Don't abuse zoom for a "move".
- Keep characters/objects visible; no awkward crops or clips through geometry.

## Depth of field (DOF)
- Background DOF on for ~⅓ of shots (blur σ720 ≈ 2–4 → blurb ~3× @1080p).
- Foreground blur: rare (~20% of shots) but strong (σ720 3.7–7.8).
- Rack focus: whole-frame blur pulses (σ 3.6–5) of 0.3–1 s at some cuts; never
  inside a static shot.
- `dofAt(f)` returns `{focus, cinf}`; focus = distance to the subject; `cinf=0`
  disables DOF. Use `cinf` ~5–14 for intentional depth.

## Kicks / impacts
- 6–7 frame kicks; offset flips sign every frame (~3 px), gone in 6–8 f.
- Roll ±0.1–0.35°; the push-in stops at the hit.

## Motion economy
- Prefer a few meaningful glides over constant motion.
- A big reveal = one wide glide or one hard cut, not a whip.
- Roll moves ~3/10s, ~3.6° over ~18 f.

## Teleport / cut rules
- If a camera change would need a >25 px/f move for >1 f of the transition, CUT.
- Time jumps use a **6-frame white flash** + a cut (not a long glide).

## Sweep instead of cut (soft transitions)
Prefer a **camera sweep** (a smooth glide that arcs between framings) over a hard cut for
most scene-to-scene changes, especially inside one continuous location. Hard cuts read as
jarring; sweeps feel cinematic and keep the eye oriented.
- A **sweep**: a short (18–40f) glide, use `path` spline for an arcing movement between
  the two framings — like the camera turning to follow the story.
- Use a hard cut ONLY when: (a) it is an intentional stylistic rhythm (a quick-cut montage
  is fine, but keep it short), or (b) a time jump / flash, or (c) a memory dissolve.
- **Memory transitions:** freeze 0.5s → soft cross-dissolve (never a cut). See GRADE.md
  for the memory grade.
- Rule of thumb: if a cut feels "too instant", turn it into a sweep.

## Wide-shot grounding check (visual QC)
When the shot is a WS/pull-back/zoom-out:
- [ ] The frame has **layered content outside the set** (no flat grey fog/void).
- [ ] At least 2 depth layers visible beyond the room (e.g. near props, mid streetscape,
      far skyline/hills).
- [ ] Distant layers are tinted toward the horizon/fog colour (aerial perspective).
- [ ] The ground visibly continues beyond the set edge.
- [ ] No large region is flat ~50% grey.
- Reference: `reference/ENVIRONMENTS.md` → WORLD-GROUNDING.
