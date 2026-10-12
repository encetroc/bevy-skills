---
name: "bevy-image-target-ui-picking"
description: "Test clickable Bevy UI rendered to an image target; fix Interaction buttons that work in windows but not in headless render-to-texture playtests."
version: 2
created: "2026-10-07"
updated: "2026-10-07"
---
## When to Use
Use when a true windowless Bevy verifier needs to exercise UI clicks on the game's actual image-target camera. Verified with Bevy 0.19.1; consult matching source before applying to other versions.

## Procedure
1. Read the version-matched examples/ui/render_ui_to_texture.rs and bevy_ui/src/focus.rs. In 0.19.1 legacy Interaction only handles Window targets; use Pointer<Press>/Over/Out observers for image-target-compatible controls.
2. Attach UI roots with UiTargetCamera pointing at the real game camera. Mark decorative text/icon children Pickable::IGNORE so their parent receives hits. Send the intended action from the Press observer through a game message; track hovered overlay identity to suppress ground click-through.
3. Spawn a custom PointerId. Drive PointerInput Move/Press/Release with Location.target normalized from the camera image target and pixel positions. Send moves and button edges on separate frames through PickingSystems::Input, preserving the real picking/backend/observer path.
4. For games also consuming ButtonInput or cursor resources, inject those after InputSystems using the same scripted coordinates and button timing. Use an optional real-window viewport sync plus a shared viewport resource rather than spawning a fake Window.
5. Assert gameplay results after game consumers, then capture the real target through Screenshot::image. Advance stages only after ScreenshotCaptured saves a checked PNG. Inspect arrows, HUD glyphs and edge entry positions visually.

## Pitfalls
- Setting Interaction::Pressed directly proves only a handler fixture, not clicking or picking.
- Default Bevy font may lack arrow, middle-dot or Unicode minus glyphs; use supported text or geometric UI icons and inspect captures.
- Use a viewport margin that accounts for the capsule's projected full height and edge-overlay dimensions; a center-only viewport assertion misses occlusion.
- When testing walk-before-screen-transition clicks, start away from the requested exit. A preceding transition can leave the player exactly at that exit, making an immediate transition correct. Order journeys to avoid this and assert both unchanged screen coordinates and an active destination immediately after the click, then the final adjacent screen after sufficient simulation ticks.
## Verification
1. With DISPLAY and WAYLAND_DISPLAY unset, assert zero Window entities, exact UI action results, no leaked ground destination, and completed fresh PNGs.
2. Inspect before/after images for readable controls and player visibility; record adapter/backend and unsupported OS/audio coverage.