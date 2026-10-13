# Bevy 0.20 coordinate consumers

The forward/inverse projection mathematics is unchanged by the release; migrate the consumers without changing the coordinate contract.

- Translate naga_oil-preprocessed shaders to WESL and rename them `.wesl`; directive-free WGSL still works. Update imports to path-based modules, conditionals to `@if`, shader constants to `constants::NAME`, and embedded registration/URIs together.
- Mesh bounds use `MeshAabb::get_aabb`, replacing `compute_aabb`. Refresh bounds after changing vertices before comparing picks with shader-space coordinates.
- Pointer observers now use flat events such as `On<PointerPress>`. Read `event.pointer.id` and `event.pointer.position`; use `event.pointer.location()` when target/location metadata is needed.
- Shape and curve modules have moved to `bevy_shape` and `bevy_curve`. Check imports/features in the target's source when mapping helpers depend on them.

Completion: run round-trip/seam tests and compile the actual WESL path in a rendered picking/placement scene. A successful CPU test does not establish shader agreement.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
