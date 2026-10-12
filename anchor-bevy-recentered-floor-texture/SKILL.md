---
name: "anchor-bevy-recentered-floor-texture"
description: "Fix Bevy floor textures that look stationary during camera scrolling because the finite ground mesh follows the camera."
version: 1
created: "2026-10-07"
updated: "2026-10-07"
---
## When to Use
Camera and recentered floor move together, making a repeating texture screen-locked.

## Procedure
1. Inspect the plane's UV axes and confirm camera and ground displacement match.
2. Test a fixed world point's local UV transformed by the actual animated ground material at intermediate and final frames; it should stay constant.
3. For an axis-aligned Plane3d of size S, set StandardMaterial.uv_transform to Affine2::from_translation(Vec2::new(ground.x, ground.z) / S) after ground movement.
4. Set both image sampler address modes to Repeat, preserving filtering; rerun tests and Clippy.

## Pitfalls
- Adjust the formula for rotated or scaled meshes and different UV layouts.
- Default clamped addressing streaks when compensated UVs leave 0..1.

## Verification
1. Regression fails before compensation and passes after.
2. Fixed world-point UVs remain unchanged during and after recentering.