# WORLD BACKDROP — the "keep the grey off" agent (world plotting)

**Owner: Agent 8 — World/Backdrop Builder.** This agent is responsible for everything the
camera can see OUTSIDE the main set (the room/street/stage). It does NOT fill the props
inside a room (that's Agent 4). Its one job: make sure no wide shot, pull-back, or zoom-out
ever shows a flat grey void.

---

## The rule (the whole point)
> **If the camera can see beyond the set, there must be a real, layered world there.**
> A pull-back showing fog/sky/void is a bug, not a stylistic choice.

## What this agent does
1. Reads the shot plan and finds every shot where the camera can see outside the set
   (WS, pull-back, zoom-out, crane-up, any shot whose framing includes the room's edges
   or beyond).
2. Builds the **backdrop world** behind/around the set: ground continuation, rolling
   hills, a skyline / town / forest silhouettes, mountains, sky gradient, and distant
   atmospheric layers.
3. Tints distant layers toward the fog/horizon colour (aerial perspective) so they sit
   in the scene.
4. Ensures the world is visible through whatever openings exist (windows, doors, open
   sides, or a purpose-built back-wall window for pull-backs).

## Build recipe (from old kit `world.ts`/`build.ts`)
- **Ground continuation:** a large plane extending beyond the room (60×60+ m), coloured
  to match the ground, so pull-backs don't cut to void.
- **Hills:** half-buried spheres (r ≈ 5–8) in two depth layers (near + far tint).
- **Mountains (far):** cone + cone-snowcap at r ≈ 520–720 (only for outdoor/exterior
  world shots).
- **Skyline/town:** repeated house tiles (cylinder/box bodies + roof cones) at the
  visible horizon; tint toward fog colour.
- **Trees:** scattered trunk + crown spheres for depth.
- **Sky:** 4-stop vertical gradient (see ENVIRONMENTS.md) — never a flat grey band.
- **Fog:** = horizon colour; it should haze distant layers, not fill the void with grey.

## Visibility
- If the set is a room, give it a **window opening** the wide shots can see through
  (a large back-wall window is the common fix), or make the side/back walls low enough
  that the pull-back reveals the world.
- The backdrop must sit behind those openings in the camera path.

## QC checklist (run on every wide/pull-back still)
- [ ] The outside shows layered content (≥2 depth layers beyond the room).
- [ ] Distant layers are tinted toward horizon colour — NOT flat mid-grey.
- [ ] The ground visibly continues beyond the set edge.
- [ ] No large region is flat ~50% grey.
- [ ] Sky has a gradient (not one flat colour).
- [ ] The world is visible through the intended openings.

## Failure you are preventing
"Camera zooms out → grey everywhere." Exactly what this agent exists to stop.
