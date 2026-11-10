# CHARACTERS — Design, Expressions & Reaction Control (Agents 1 & 3)

## Making a cast that is genuinely distinct (lesson learned)
- Characters must NOT be just color swaps of the same template. That reads as "swapped
  heads" and fails. Distinguish by: **body build** (adult/stout/kid), **silhouette** (long
  coat, cape, toga drape, armor, flared dress, apron/overalls), **hat/hair style**,
  **facial hair**, and **head seed** (each character gets a unique blob seed so faces differ).
- Engine support: `stout` (boolean) + a `kid` build (smaller head, shorter torso/legs); a
  `coat` (extends below the hips), `cape` (back panel), `toga`, `armor`, and `dress`/`apron`
  each change the body outline. Use them — not just `tunic` colors.
- **Belt rule:** give each outfit `belt: false` unless a belt is an intentional design part.
  The default waist/ belt reads as clutter on historical/plain costumes. In the renderer the
  waist band is gated by `O.belt !== false`.
- Two curated casts ship in the project (see `shorts-xqivo-silence/characters_40/`):
  - `old/` — 20 characters in the `claude-animated-shorts` cast grammar (soldier/officer,
    toga, townsfolk with tophat/bonnet dresses, kid-cap build, parent, workers, + extras).
  - `new/` — 20 modern characters (CEO, artist, cyber, teacher, gym, vintage, robot, punk,
    witch, waiter, runner, cop, chef, geek, hippie, etc.).
- Honest limit: the renderer's torso is a shared bean template; distinctness comes from the
  silhouette overlays + build + head/hat, not from fully unique body construction. Truly
  bespoke bodies require hand-drawn per-character shapes.

Source: merged from `claude-animated-shorts/references/reports/characters.md` and the
new kit's `engine/roman.js` (cast, FACE, poses, faceFront/faceSide).

## Head shape (hand-drawn feel)
- H/W ≈ **0.93** (rounder than perfect circles).
- Egg taper ≈ −0.125 (narrows toward chin).
- Crown n_top ≈ 1.7 (slightly pointed), jaw n_bot ≈ 2.26 (fuller).
- Asymmetry ≈ +0.10; slight tilt; subtle contour wobble (~1% W).
- Eyes/mouth shift ~+0.08 W toward the side the character faces.
- No blush, no catchlights.

## Body
- Adults ≈ 3.5 head-heights tall (head 1 : chin→torso 1.1 : legs 1.4); kid 2.85 HH;
  stout 3.47 HH.
- Bean-shaped torso (widest 60–70% height, round bottom), width ≈ 0.80 W adult.
- Legs = single ink stroke ≈ 0.055 W (no knees, slight curve); feet short strokes
  0.15–0.25 W long × 0.07 W; A-stance feet ~0.45 W apart.
- Arms ≈ 0.05 W hanging outside the bean; hands = ink dots 0.08–0.15 W.
- Torso shading: lit→shadow 1.0→0.55–0.66 smooth gradient; optional inner rim light.
- Hats sit low (cover top 25–40% of head, base sags like a "smile", tilt with head).
- Avoid chibi: limbs thin, not 5× too thick.

## Build table (design units, 600×1000 canvas, soles y=975)
| | head a/b | torso HH | legs HH | width |
|---|---|---|---|---|
| adult | 118/111 | 1.10 (244) | 1.40 (311) | 0.80 W |
| stout | 112/108-ish | 1.15 | 1.32 | 0.92 W |
| kid | 112/108 | 0.85 | 1.00 | 0.72 W |

## Line weights (u = 1% W)
- head/torso w0 = 5u; hats/props 0.85×w0; legs 5.8u (mix INK 0.55); feet 7.5u thick
  × 16u long; arms 5.2u (mix INK 0.5); hands dot r 5.5u; brows tapered; eye 3.8u;
  mouth 4u; clothing internals 1.5–1.8u. Min width ≥ 1.5/k units (texture tier k=0.4).
- Outline thickness model: `w(θ)=w0·(1+0.45·cos(θ−θthick))` — heavy at bottom/shadow side.

## Face geometry
- Eyes 0.26 W apart on centre line; default eye = lidded half-disc (deadpan).
- Shock eyes = white circles 0.21–0.24 d, almost touching.
- Brows at y −0.20 W, tapered 5u→2.5u. Mouth at +0.20 W.
- Features slide 0.07–0.10 W toward the facing side. No blush, no catchlights.

## Expressions (FACE)
Define with `{eye:[left,right], brow:[left,right], mouth}`:
- neutral: lid/lid, brow [0,-1], dash
- stern: lid/lid, brow [3,-2], frown
- grin: happy/happy, brow [-3,-3], grin
- laugh: squeeze/squeeze, brow [-4,-4], laugh
- shock: shock/shock, brow [-7,-6], O
- pained: squeeze/lid, brow [-3,2], wave-ish
- dazed: spiral/spiral, brow [-3,2], wave
- nervous: small/small, brow [-3,2], wave
- snore: dash/dash, brow [0,-1], o
- stubborn: lid/lid, brow [4,-3], frown
- squint: slit/slit, brow [3,-2], dash

## Reaction control (Agent 1) — decision rules
- **Surprise/shock beat** → big readable reaction: `shock`/`pained`, wide eyes,
  open mouth, body settle; lands **0–3 frames before** the event/word.
- **Normal beat** → normal face + neutral body. No exaggerated grin on a calm line.
- **Punchline/relief** → `grin`/`laugh` once the beat resolves.
- If a character is reacting to something OFF-frame (sound), the reaction must still
  be justified by the script — never random.

## Blinks (alive, natural)
- Every 0.5–2.3 s; 4 frames: half / shut / shut / half.
- Plus **one blink on every expression swap** (softens the change).

## Arm/body poses (roman.js `arm()`)
- hang, spear, point (finger), up, wave (animates), hip, belly (front), temple
  (swirl/cuckoo), lift, shield (front left), cross (front), reach (uses lift),
  cheek, thumb.
- Every pose should read clearly; hands are ink dots.

## Front vs side (half-face)
- `faceFront` = full face (both eyes, symmetric-ish).
- `faceSide` = true profile: one eye, nose line, mouth to the side. Use with the
  side body when the camera is beside the character.
- When a character turns, shift features to the facing side (don't just mirror).

### Side-face eye orientation (lesson learned)
- In profile (`faceSide`), the closed/laughing eye kinds (`squeeze`, `happy`) draw as a
  chevron whose direction is set by `side`. Hard-coding `side=1` made them read **backwards**
  (like the eye is on the back of the head). Fix: mirror those kinds in side view
  (`eyeSide = (kind==="squeeze"||kind==="happy") ? -1 : 1`), so a right-facing profile bows
  forward/down like a real happy eye. Other kinds (neutral/shock/lid) keep `side=1`.
- `faceSide` MUST receive the actual `expr` from opts — never hard-code `'stern'`; a
  hard-coded expression makes every profile reaction show the same face.

### Rendering character close-ups (lesson learned)
- Do NOT tune the 3D camera to zoom into a character's face — it is unreliable (too tight
  crops the face, too loose makes it tiny, and the result hides the character).
- Reliable method: render the FULL body, then **crop to the head with PIL** (or use a
  measured head-band transform on the card canvas). A head-and-shoulders band (design y
  ~100–700) reads best: features clear without over-zooming.

## Smooth arm/hand motion (never teleport)
A pose change must NEVER cut from one arm pose to another instantly — it reads as a
teleport. Blend the arm control points from the previous pose to the new pose over
~10 frames (smoothstep), so hands visibly MOVE to the new position.
- Implement: keep the previous pose (`prevL`/`prevR`) and a fade timer (`armT`, 0→1
  over ~10 frames); lerp the shoulder/elbow/hand points between the two poses.
- Apply to every pose transition (type→rest, reach→wave, etc.). The hands travel,
  they do not teleport.

## QC checklist (Agent 3)
- [ ] Heads are round-ish, not perfect ellipses; outlines have bottom weight.
- [ ] Reaction matches the beat (calm = calm, shock = shock).
- [ ] Blinks present; no frozen eyes.
- [ ] Hands are ink dots; no oversized blobs.
- [ ] Side view actually reads as profile, not a mirrored front.
