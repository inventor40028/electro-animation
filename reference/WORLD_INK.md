# WORLD INK — black outline pass (from old `outlines.md`)

## Reference (measured)
- ~74% of geometry edges carry ink: silhouettes vs sky, depth jumps, convex creases
  (box corners, table/wall tops, window frames), contacts (object on floor).
- NO ink on wall/floor joins, baseboards, or sea horizons.
- Width ≈ **3 px @1080p**, near-black #0b0a0c, crisp (anti-aliased only), nearly constant
  with distance (~3.4 px at 6 m → 2.2 px at 90 m). Applied BEFORE DOF so it blurs with the
  background. Never on character cards.

## Recipe (world-only normal+depth pass)
- Depth edges = signed 2nd difference of 1/z:
  `s = (1/z1 + 1/z2 − 2/zc)·zc` (exactly 0 on planes), near side only,
  `smoothstep(depthLo 0.02, depthHi 0.05, −s)`.
- Creases = normal-angle test when `|Δz| < 2% z`:
  `1 − smoothstep(cos65°, cos50°, n0·nq)`, width ×0.64; concave folds +0.7 (contacts).
- Width: sampling radius 1–6 px steps, continuous coverage `clamp(w+0.5−d, 0, 1)`.
- Fade: `farOpacity 0.75` at 90 m, full fade 150–450 m. 3×3 tent AA.
- Colour #0b0a0c, strength 1.0, blend in display gamma.

## Order
`bloom → ink → dof → tone`. **Never merge ink into the DOF effect** (it erases the lines).
Skip via `NOINK` flag on cards/fx/sky/clouds/smoke/Line/Points.
