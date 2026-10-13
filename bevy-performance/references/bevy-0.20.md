# Bevy 0.20 profiling candidates

Use these as measured hypotheses, not blanket optimizations or release-benchmark promises.

- `#[component(summary_tick)]` allows whole-column skipping in sparse `Changed<T>` queries at increased mutation cost. Compare idle/sparse and write-heavy workloads.
- Bulk `despawn_all<F: QueryFilter>` reduces cleanup overhead; `DespawnOnEnter` / `DespawnOnExit` already benefit. Measure transition hitches separately from steady-state frame time.
- Weak ordering can improve parallelism, but only for dependencies fully represented by ECS access. Several built-in rendering/UI sets changed to weak chains; preserve strict edges for hidden channel/atomic dependencies.
- Retained UI extraction rebuilds changed nodes instead of all nodes (transparent elements remain an incomplete migration). Inspect idle writes and custom render hooks; use `ExtractedUiNodes::changed` rather than assuming wholesale re-extraction.
- Solari retains scene management and disables ReSTIR by default. Compare quality and cost with explicit `SolariLighting::restir` settings; many-light motion can lose shadow quality without it. Record cache size, samples, accumulation, bounce count, DLSS version, and adapter.
- Sprite backend unification can change performance and equal-Z ordering. Repeat representative overlap/input/capture tests when measuring it.
- Texture processing now generates mipmaps with BCn/ASTC compression; choose universal/UASTC for cross-platform transcoding when needed. Measure load time, memory, and distant aliasing, not just file size.
- Mesh shaders are an advanced platform-specific option, not a portable `StandardMaterial` replacement. They are unsupported on web; confirm hardware support before an A/B comparison.

Completion: matched captures and repeated workload measurements with intentional 0.20 defaults recorded. Release speedups are not measurements of this application.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
