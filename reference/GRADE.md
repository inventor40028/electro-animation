# GRADE — colour & tone (from old `grade.md`)

## Memory grade (nostalgia) — per-scene rule
A memory scene (e.g. a daytime "flashback" to the same desk) must feel like a memory,
not just a brighter copy of the present. Apply this distinct grade:
- **Warm, lifted highlights** — screen whites shift toward warm cream (#fff4e0 → #ffe8c4).
- **Softer contrast** — reduce contrast ~15%; lift blacks (p5 ≈ 0.18–0.22), never crush.
- **Increased golden warmth** — warm the brown/shadow tint (toward #d9a464); overall cast
  reads "sunshine remembered", not clinical daylight.
- **Slight bloom on the bright window** — the memory's light source glows (emissive
  sources only), like sunlight through a warm veil.
- **Vignette slightly stronger** (k 0.14–0.18) — corners fall off gently = "old photo".
- **Lower saturation on cool colors, keep warm colors rich** — so the memory reads warm
  even though the present is cooler.
- Optional: a faint dust-grain / warm haze overlay (very subtle; do NOT add film grain
  unless requested — reference has none).

## Memory transitions (dissolve only — no hard cuts into/out of a memory)
- Freeze 0.5s → **soft cross-dissolve** (1s) to the memory grade, and the reverse on the
  way out. Never cut straight into a memory; the dissolve IS the "memory" cue.
- Keep the same camera line across the dissolve so only light+grade+props change.

## Targets (per-frame medians)
- Day: Y' p50 ≈ **0.571**, p99 ≈ 0.967, non-ink p5 ≈ **0.234** (no crushed blacks),
  lit chroma C* p50 ≈ 34 / p90 ≈ 55, colourfulness ≈ 83, vignette corner/centre ≈ **0.86**
  (brighter corners are wrong).
- Black point: warm (35, 19, 20). White point: (254, 251, 246).
- Golden: Y' p50 ≈ 0.516, violet shadows (dark-zone b* −6, not brown), ~25% violet/blue/
  magenta/rose hues.

## Fitted grade
- Tone curve: monotone cubic Hermite through 7 knots x = 0, .1, .25, .5, .75, .9, 1.
  - Day y-knots: 0.0345, 0.1828, 0.3812, 0.6605, 0.8814, 0.9631, 1; tangents 1.483→0.369.
  - Golden: 0.0311, 0.1877, 0.4017, 0.6754, 0.8750, 0.9582, 1.
- Saturation per zone (shadow/mid/high): day 0.983/1.273/1.457; golden 0.832/0.880/1.706.
- Three-way tint (Cb, Cr):
  - Day: shadow (−0.0291, +0.0411) warm-red; mid (+0.0376, −0.0104) cool; highlight
    (+0.0144, −0.0047).
  - Golden: shadow (+0.0528, +0.0004) blue-violet; mid (+0.0583, −0.0003); highlight 0.
- Vignette: day k 0.10 / r0 0.45; golden k 0.16 / r0 0.40.

## Shader core
```
c = y + s(c−y) + wS·tintS + wM·tintM + wH·tintH;
r = |uv·2−1|·0.707; c *= 1 − vigK·smoothstep(r0, 1, r);
```
Runs AFTER the neutral tone map. `amount`: day 1.0 / golden 0.8.

## What grade can't fix (do it in materials)
- Cast shadow lit/shadow ratio ≈ **2.8–4.4:1** (median ~3.5 day, 4 golden); shadow L* 42–55
  blue-violet b* −20 to −30. Add a shadow-tint term to the two-tone light.
- Albedo lit targets: red #e85648, orange/wood #e3b684 (L* 77), yellow #d3c677, green
  #7fbc4e/#5ab04c, azure #93c6f9, blue #7980b0.
- Coloured darks L* 16–22: navy #262c51, aubergine #2e2439, plum #3d2530, oxblood #4d2725.
- Ink ≈ 2–2.6% of pixels. Golden needs violet sky/shadow + red/rose accents.
- Lower character emissive to ≤ 0.12.
