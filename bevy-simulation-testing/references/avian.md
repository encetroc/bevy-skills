# Windowless Avian physics tests

Resolve both Bevy and Avian versions/features. The previous Bevy 0.19 fixture composed `MinimalPlugins`, `AssetPlugin`, `MeshPlugin`, the Avian physics plugin group, and the feature under test. Check current plugin prerequisites rather than treating this list as universal.

Use manual simulated time and initialize plugins before the first update; the recorded fixture required `app.finish()` to avoid Avian startup panics. Follow readiness and cleanup requirements for the target release.

Query authoritative physics position/velocity after a known number of fixed ticks or a bounded settling condition. Assert gravity, settled height, and wall containment with scale-appropriate tolerances.

For controlled repositioning, follow the release's synchronization rules. The prior fixture updated both Avian `Position` and Bevy `Transform` to avoid stale collider state. Such setup proves neither gameplay movement nor input mapping.

Run physics-only tests without a renderer/window. Visual collision alignment, contacts on tessellated surfaces, and presentation interpolation need separate evidence.
