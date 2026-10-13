---
name: bevy-rendering-and-materials
description: "Implement Bevy camera composition, render-to-texture, custom materials, procedural surface patterns, or diagnose visual rendering defects."
---
# Rendering and materials

## Version gate

Resolve the project's Bevy version/features first. For Bevy 0.20 work or an upgrade to it, read [0.20 task checks](references/bevy-0.20.md) before implementation; retain version-matched APIs for older projects.

## Procedure

1. Resolve the Bevy version, render features, and backend. Start from the matching camera/material example; identify whether the change belongs in the main world or render world.
2. **Choose the smallest render seam.** Use camera targets, layers, and ordering for multi-pass composition. Use a custom material or material extension for surface shading. Enter render-graph internals only when those seams cannot express the effect.
3. Define the geometry budget, coordinate space, asset ownership, and Rust/shader binding contract. For shader-generated grids or checkerboards, read [surface patterns](references/surface-patterns.md).
4. Implement one representative scene path. Preserve transform/visibility propagation and explicitly target HUD/UI at its camera. Done when the runtime compiles the actual shader and renders the intended objects.
5. Verify ordinary and stress configurations, toggles, close/wide views, and target-backend behavior. Inspect fresh captures; Rust compilation alone does not establish shader or visual correctness.

## Reference branches

- **Recentered geometry:** read [texture anchoring](references/texture-anchoring.md) when moving a finite mesh should preserve a world-anchored repeating pattern.
- **Shadows or disappearing overlays:** read [visual diagnostics](references/visual-diagnostics.md) for controlled shadow comparisons and rendering-component visibility checks.
- **Renderer internals:** follow the release's extraction/render-phase examples. Treat render-world data ownership, schedule phases, and frame latency as a separate boundary.
- **Render-to-texture:** retain the image handle and check target format, dimensions, camera layers/order, and UI camera selection. Use the release's ordinary render-to-texture example before introducing custom readback.

## Completion

The actual render path runs without shader/asset errors, captures show the expected effect, and any performance claim has measured frame evidence separate from geometry counts.
