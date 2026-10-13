# Windowless setup and official sources

Resolve exact dependencies from the game's manifest/lockfile. Use tagged Bevy source, versioned docs.rs, or the exact Cargo registry source; development examples may describe another API.

## Source selection

Inspect the matching release's:

- `examples/app/headless.rs` for simulation-only runs.
- `examples/app/headless_renderer.rs` for windowless rendering.
- `examples/app/externally_driven_headless_renderer.rs`, when present, for manual lifecycle/device progress.
- `examples/window/screenshot.rs` and screenshot implementation for image-target capture/completion.
- `examples/ui/render_ui_to_texture.rs` for camera-associated UI and image-target pointer input.
- Input plugin implementation and time strategy docs for scripted-input ordering and manual delta.

Example URLs follow `https://github.com/bevyengine/bevy/blob/v<VERSION>/examples/...`; confirm the path exists in the selected release.

## Bevy 0.19.1 configuration fragment

The retained smoke crate pins 0.19.1. This is a version-specific starting point, not a standalone game:

```rust
app.add_plugins(
    DefaultPlugins
        .set(WindowPlugin {
            primary_window: None,
            exit_condition: ExitCondition::DontExit,
            ..default()
        })
        .disable::<WinitPlugin>(),
)
.add_plugins(ScheduleRunnerPlugin::run_loop(
    Duration::from_secs_f64(1.0 / 60.0),
))
.insert_resource(TimeUpdateStrategy::ManualDuration(
    Duration::from_secs_f64(1.0 / 60.0),
));
```

Use imports from the matched example. Create an `Image::new_target_texture` and insert `RenderTarget::Image` on the game camera. Retain its handle and preserve layers/postprocessing. Attach UI with `UiTargetCamera` where required.

Runner pacing and simulated delta are separate. A manual runner must complete plugin readiness, finish/cleanup, main/render updates, and GPU progress according to the target lifecycle.

Replace primary-window/cursor assumptions with viewport/input providers. A fake window defeats the zero-window assertion. Off-screen rendering still requires a compatible graphics adapter; keep adapter, asset, and shader failures visible.

For resource keyboard injection, run after transient input edges are cleared and before gameplay consumers; raw messages arrive before input processing. Verify sets in target source. Action abstractions need their own supported seam. Pointer tests need a pointer identity, target-associated location, coordinates, and move/press/release messages through picking, not assignment of the final interaction state.
