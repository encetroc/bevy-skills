---
name: bevy-performance
description: "Diagnose Bevy frame-time regressions, idle change-detection churn, or rendering and geometry cost."
---
# Frame-cost profiling

## Procedure

1. **Fix the workload.** Use the application's real startup scene/plugins, camera, resolution, and representative input. Record Bevy/features, adapter/backend, build profile, and scene counts. Serialize runs sharing a GPU.
2. Separate end-to-end wall-frame intervals from CPU update timing and available GPU diagnostics. State whether presentation is included; an image-target benchmark omits window presentation.
3. Wait for assets/pipelines and collect a declared sample count sufficient for stable median/tail estimates (40 is a starting point, not a statistical guarantee). Repeat runs to estimate noise. Done when the baseline is reproducible.
4. Instrument suspected churn: stationary camera transform/projection changes, modified mesh/material assets, geometry counts, or subsystem timings. Make an idle-write monitor detect an intentional write before trusting a zero result. Check installed plugins before attributing work to a backend.
5. Change one variable per A/B comparison. Use equality guards before mutable access for idle-write fixes. For rendering settings, test antialiasing, shadows, or shader work independently; retain only improvements larger than measurement noise.
6. Re-run input journeys and inspect matched captures for fine detail, text, silhouettes, contact alignment, and toggles. Report commands, sample statistics, tradeoffs, and coverage.

## Interpretation

- A fast subsystem or CPU update does not establish FPS; rendering, synchronization, pacing, or presentation may dominate.
- Software adapters support comparisons on that adapter, not hardware FPS promises.
- Geometry reduction and frame-time improvement are separate measurements.
- Compare antialiasing modes with the target release's example; a faster mode can soften important detail.
- Preserve collision and gameplay constraints while optimizing their visual representation.
