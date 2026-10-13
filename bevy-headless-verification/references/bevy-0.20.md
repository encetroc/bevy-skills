# Bevy 0.20 render-headless checks

## Version boundary

The bundled smoke crate and its configuration fragment remain pinned to 0.19.1. They are infrastructure diagnostics, not a compiled 0.20 recipe or 0.20 compatibility result. For a 0.20 game, resolve windowless setup, screenshot completion, and input injection from its exact tagged examples/source before copying a fragment.

## Runner integration

- Preserve zero windows, actual game plugins, checked saves, and per-checkpoint identities while updating APIs. Custom render extraction now needs target app labels (for example `#[extract_app(RenderApp)]`); extraction infrastructure lives in `bevy_extract`.
- Several render/extraction/UI set chains are now weak. Capture acknowledgements or scripted commands crossing channels/atomics need strict producer/consumer ordering where ECS access does not express the dependency.
- Migrate pointer observers to flat events such as `PointerPress` and check `pointer.id`, `pointer.position`, and `pointer.location()`. Update widget routing with the 0.20 button example; legacy `Interaction` is deprecated.
- UI extraction is retained. Custom readback/render hooks must not assume every node is recreated each frame; account for the changed-node set.
- For BSN scenes, wait for hierarchy `Ready` when needed, then independently wait for assets/pipelines and bounded render stabilization.
- Ensure a custom fallback error handler cannot log a panic and let the verifier report success. Promote application/capture errors to failed checkpoints.
- Capture comparisons must record intentional changes to transmission, mixed-HDR compositing, Solari ReSTIR, font scale, or sprite Z-order defaults.

Completion: a 0.20 renderer-backed game run with checked, inspected captures. A 0.19.1 probe pass or documentation review alone cannot satisfy this.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
