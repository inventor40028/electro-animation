# ELECTRO ANIMATION — Reference Implementation

This is the old kit's **complete, canonical implementation** ("The Bridge That Fell for a
Song") copied from `claude-animated-shorts/templates/bridge/`. It is the *actual code*
behind every measured number in `reference/`. Use it as the authoritative reference when
building a new short.

## What each file is (the render chain)

| File | Role | Maps to |
|---|---|---|
| `story.ts` | Timeline, 15-shot plan, cameras (CAMS), light presets, DOF, world state | `reference/CAMERA.md`, `ELECTRO_ANIMATION.md` §2 |
| `world.ts` | Sky shader, fog=horizon, river water, per-frame posing, sun frozen per shot | `reference/ENVIRONMENTS.md` (sky/water) |
| `build.ts` | Town/houses/props, sight-line culling, flag/banner cloth | `reference/ENVIRONMENTS.md` |
| `toon.ts` | Character drawing API: Pen, faces (14 expressions), bodies, hats, blocking | `reference/CHARACTERS.md` |
| `cast.ts` | Sprite pool, billboards on NOINK, anchor/helper FX, foreground props | `reference/CHARACTERS.md` |
| `lights.ts` | `LIGHTS`/`LOOK` presets (day/golden exact values) | `reference/ENVIRONMENTS.md` lighting |
| `shade.ts` | Two-tone light shader (lit/shade/sky fill/rim) | `reference/GRADE.md` "what grade can't fix" |
| `cardshade.ts` | Card shader (side gradient, top boost, lift) | `reference/CHARACTERS.md` |
| `ink.ts` | World outline pass (depth jumps, creases, NOINK) | `reference/WORLD_INK.md` |
| `lensblur.ts` | Thin-lens DOF gather pass | `reference/CAMERA.md` DOF |
| `grade.ts` | Tone curve, per-zone saturation, three-way tint, vignette | `reference/GRADE.md` |
| `Bridge.tsx` | Composer order: bloom → ink → lens pre/main → tone → grade | `ELECTRO_ANIMATION.md` §2 |

## How to use it
- This is a **Remotion/three** implementation (TypeScript, `@react-three/fiber`,
  `@react-three/postprocessing`). It is the *reference for how the look is achieved*, not
  necessarily the exact runtime for a new project.
- The new kit (`engine_reference/`) provides the lighter, runnable JS engine. Use
  `engine_reference/` to run; use `implementation/bridge/` to understand the exact
  numbers/shader recipes when you need the full look.
- When you need a precise value (a light preset, a shadow ratio, an ink width, a tone
  knot), read the corresponding file here first — it has the real, validated numbers.

## Proven
This implementation scored (from the old kit's own Opus-based review): colour day 7,
lighting 5–6, ink 6, characters 7, depth 6–7, composition 6–7. Treat it as the target
quality bar.
