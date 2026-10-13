# World-anchored repeating textures

Use when a camera-following finite surface makes its texture appear screen-locked.

Define the invariant first: evaluate the animated material's transformed UV at a fixed world point during and after mesh recentering. It should remain unchanged, modulo the repeating texture's period.

For an axis-aligned plane of size S with the expected X/Z UV layout, compensate its translation with:

```rust
material.uv_transform =
    Affine2::from_translation(Vec2::new(ground.x, ground.z) / size);
```

Confirm UV directions/signs on the actual mesh. Derive compensation through the world-to-local transform for rotation, scale, or another UV layout. Apply it after ground movement.

Use repeat addressing on both texture axes while preserving intended filtering; compensated coordinates leave the unit interval. Verification requires the invariant to fail before the fix and pass at intermediate as well as final positions.
