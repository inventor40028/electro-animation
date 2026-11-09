# ENVIRONMENTS — Research, Detail & Event Logic (Agents 4 & 6)

Source: merged from the old kit's world/build/paint/textures reports and the new kit's
`main.js` world builder + `paint.js`.

## WORLD-GROUNDING — no grey void on wide shots (hard rule)
A camera pull-back/wide shot must never show just fog/sky around the set. If the camera
can see beyond the room, the outside must be a **real, layered world**:
- **Immediate surroundings:** skyline silhouettes (town roofs, trees, hills, mountains)
  at the visible horizon, tinted toward the fog/horizon colour (aerial perspective).
- **Ground continuation:** the floor should extend or meet a visible ground plane outside
  the room — not cut off into void.
- **Window/scene backdrop:** if a window or open side is visible, the world beyond it
  must be built (sky + horizon + distant shapes), never empty fog.
- **Fog is atmosphere, not emptiness:** it should haze *distant* layers toward the horizon
  colour — not fill the whole outside with one flat grey.
- **Checklist (run for every wide/pull-back shot):**
  - [ ] The outside of the room has visible, layered content (≥2 depth layers).
  - [ ] Skyline/distant shapes are tinted toward horizon colour, not mid-grey.
  - [ ] The ground visibly continues beyond the set edge.
  - [ ] No large area is flat ~50% grey fog.
- Reference recipe (old kit `world.ts`/`build.ts`): half-buried spheres for rolling hills,
  cone+cone mountains at r=520–720, a town of house tiles beyond, 4-stop sky gradient,
  fog = horizon colour (never flat mid-grey). Use the same for any seated interior so a
  pull-back reads as "a studio in a neighbourhood" not "a box in grey".

## Rain / weather visibility (lesson learned)
Rain must be a VISIBLE presence, not a subtle far-wall detail. Failures observed:
- Drawing dark rain streaks on a dark night window = invisible. Use bright droplets that catch
  the room light (light blue-white `rgba(215,232,248,0.85)`), and position them where the
  camera actually points.
- If the camera faces the desk (not the window), a rain FX on the far window is off-camera and
  wasted. Either (a) frame the window in at least some shots, or (b) add a visible rain sheen /
  droplet FX within the lit frame itself.
- Verify with a rendered frame: the rain pixels must be measurable (bright blue fraction
  clearly > 0.01) in the ACTUAL camera view, not just present in code.

## Real screen-recording compositing (on-device screens)
When a character "uses" a real app (typing an address, seeing a site), you can composite a
REAL screen recording onto the device's screen so it looks genuinely used:
- Ask the user for a short screen capture (5–8s, e.g. "open a new tab → type xqivo.com").
- Extract frames from the MP4 (ffmpeg) into `web/screenrec/f0000.jpg…`.
- In the renderer, show the recording on the device screen during the relevant shot (the
  character's hand + the recording together read as real use).
- Keep it quiet and natural: the recording is one extra prop, not a cutaway.

**CRITICAL (dimension-safe):** never assign the recording image directly to a
`CanvasTexture` (`screenTex.image = img`) if the image size differs from the canvas — that
triggers `GL_INVALID_VALUE: Offset overflows texture dimensions` and the texture silently
keeps its old content (a white invoice). Instead, **draw the image onto the existing
canvas** at the canvas size:
```
function drawScreen(img){ if(!img||!img.width) return; sctx.clearRect(0,0,W,H);
  sctx.drawImage(img,0,0,W,H); screenTex.needsUpdate = true; }
```
Also verify the recording frames actually contain visible content — if the capture is a
dark/loading screen (luma ~5–30), it will read as a blank screen on the device. Ask for a
bright, fully-loaded capture.

## Agent 4 — fill an underspecified environment
When a user says "an office" (or any generic place), research and populate it. Minimum
realistic contents per common environment:

- **Office/desk room:** desk, chair, computer monitor + keyboard + mouse, window (with
  sky/light), a lamp, wall clock, door, plant, paper stack, mug, poster/frame, shelf,
  ceiling + ceiling fan.
- **City street:** buildings (facade textures, windows), road + sidewalks + lane
  markings, street pole/posts, trees, sun + sky + clouds, moving cars/people/birds.
- **Forest:** trees, undergrowth/bushes, light shafts, fallen branches, critters, sound.
- **Home/client room:** desk, chair, warm walls, window, door, decor, personal props.

Production rule: build from plain 3D shapes (`tbox`, cylinders, spheres) + canvas
textures (`paint.js`). Give each environment a **light preset** and matching **SFX bed**.

## Reusable environment props (from the kits)
- **Buildings:** canvas facade texture (window grid), building mass, corner reveals,
  roof parapet + fins; far tower ring.
- **Roads:** dark strip + sidewalks + dashed lane markings; street poles + lamp heads;
  moving cars (two lanes) + people + birds as FX cards.
- **Office desk set:** keyboard (canvas key caps), mouse (shell + buttons + cord),
  mug (cylinder + handle), paper stack, plant (pot + leaf canopy), lamp (stem + bulb +
  warm point light), ceiling fan (rotating), wall clock (face + hands).
- **Glass curtain-wall house:** facade canvas (mullion grid + panels + lit floors +
  lobby door + canopy), single-sided dark mass (so the camera can fly through), roof
  parapet/fins. Only show the exterior during the transition/zoom beat; keep interiors
  open otherwise.
- **Water:** value-noise shader with bank soft edge + glint streaks + flow time; flat
  saturated vertical gradient, sparse horizontal marks, no reflections.
- **Sky:** 4-stop gradient by elevation (day: #d8ecf7→#2a7fe6; golden: #ffd08a→#8c78c4)
  + sun disc + drifting cloud cards.

## Lighting presets
- **day:** az 75 / el 48; warm-white sun; hemi cool; fog = horizon colour, 25→420 m;
  shadows Y ratio shade/lit ≈ 0.5 (keep hue); bright, saturated sky.
- **golden/sunset:** az 15 / el 9 (BACKLIT); orange sun low; violet-blue shadows
  (B ratio ~2–3); mauve upper sky, bright yellow-orange horizon.
- **evening/night:** dark blue sky; warm desk lamp turns on; window light dims; add a
  fill so the room isn't crushed black.
- **card/endcard:** near-black bg, teal logo.

## Agent 6 — action/consequence logic
- Every action needs a visible cause and effect, plus a character reaction.
- Examples: spilled coffee → liquid spreads on the desk, mug tips, steam stops,
  character reacts, maybe cleans up; time passing → clock moves, leaves fall, coffee
  dies; invoice sent → it flies/glows, client catches it, grins.

## Texture/detail targets (old kit, trusted)
- Magnification ≤ 6 px/texel at closest lens; ≤3 at working distance.
- Per-surface: deck planks lit L* 72–80; stone neutral C* ~10; grass #71b038; iron
  never below L* 19 with a bright band; water flat + cyan at horizon; clouds 3 tones,
  lavender underside, edge ramp 8–20 px.
- Roofs: no per-tile ink, joints ~3.3 px ΔL* 10.
- Texel densities: planks 1024/m (2048²), stone 512/m, cobble/grass/roof 256/m, facade
  96 px/m (cap 2048), chain 512/m, water 1024/8m near + 1024/40m far, clouds 1024×512.
- Sky: 4-stop by elevation. Day `#d8ecf7 (0°) → #2a7fe6 (30°+)`; golden
  `#ffd08a (0°) → #8c78c4 (32°+)` — interpolate with smoothstep, never mix blue→orange.
- Water shader: depression-angle gradient, near/far mark textures mixed by distance
  (25→90 m), time offset 0.35 m/s along +x; day palette wNear #0c5a8c → wGlint #e8fbff;
  golden #23306a → #ffe2b0.
- Painted seams (texture lines): colour × 0.35–0.45, alpha 0.8–0.9, ~1.2–2 px @1080p,
  never doubling a geometry edge.
