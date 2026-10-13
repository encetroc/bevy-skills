# Bevy 0.20 placement integration

The occupancy transaction remains unchanged: preview retains the original reservation; validation precedes an atomic commit.

- Update picking observers from generic `Pointer<Press>` to `PointerPress` (and the corresponding flat event types for other edges). Read `pointer.id` / `pointer.position`, or `pointer.location()` when target metadata matters. Preserve drag cancellation, release delivery, and UI/world ownership.
- Recompute moved-mesh bounds through `MeshAabb::get_aabb`, the replacement for `compute_aabb`. Exercise raycasts after vertex changes rather than just asserting occupancy.
- Preview shaders using naga_oil directives must migrate to WESL; directive-free WGSL remains supported. Test transparency, opacity restoration, and the actual shader at runtime.
- Screen-space transmission is now opt-in on `Camera3d` through `ScreenSpaceTransmission`. If previews depended on refraction, choose explicitly whether to restore it; alpha blending alone is a different effect.
- If placement feedback uses sprites, assign distinct Z levels for overlapping sprites; the unified sprite backend can change same-Z ordering. Use current picking/widget behavior rather than legacy `Interaction` assignment.

Completion: replay valid/invalid drop, cancel, UI interception, and moved-mesh picking on 0.20, then inspect both transparent previews and opaque commits.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
