---
name: bevy-headless-verification
description: "Verify Bevy gameplay with windowless rendering, scripted actions, checked off-screen captures, or visual CI evidence."
---
# Render-headless verification

Use when behavior and rendered evidence are both required. Simulation-only tests and an invisible window establish different coverage.

## Procedure

1. Resolve Bevy/features and reuse the game's existing app builder, verifier, and artifact conventions. Define named checkpoints with starting state, actions, simulation ticks, state assertions, and visible expectations.
2. **Preserve the game.** Retain real plugins, assets, camera, HUD, and handlers. Configure rendering without window entities and target the actual camera at an image. Read [setup and sources](references/official-bevy.md) before implementing the runner.
3. Seed randomness and control simulated time. Wait for observable asset/state readiness, then bounded render stabilization. Preserve the application's pipeline-compilation mode when first-use behavior matters.
4. Inject through the input layer the game consumes, ordered before consumers and relative to input processing. Preserve press/hold/release edges. Pointer scenarios need positions and the real picking path; state the mapping/OS coverage boundary.
5. Assert after consumers and request checkpoint captures. Read [capture protocol](references/capture-protocol.md) before implementing completion tracking. Advance ordinary scenarios only after checked saves; bind transient-frame captures to explicit frame identities.
6. Build separately from the runtime deadline. Run from the real asset root, with display variables unset on Linux, a frame watchdog, and an outer wall-clock timeout. Assert zero windows and record adapter/backend.
7. Verify every required capture is fresh, decodable, and correctly sized; open and inspect the images against each checkpoint's visible expectations. Complete only when gameplay assertions, checked saves, and visual evidence agree.

## Branches

- **Environment failure:** use [the bundled smoke probe](references/smoke-probe.md) to separate adapter/capture failures from game failures. Its success verifies infrastructure, not the user's game.
- **Camera framing or orbit journeys:** read [camera scenarios](references/camera-scenarios.md) for viewport and re-entry assertions.

## Report

Give scenario/actions, assertion results, commands, screenshot paths/count, version, adapter/backend, resolution, and coverage limits. Distinguish passed gameplay verification, simulation-only success, infrastructure smoke success, missing evidence, and environment-blocked runs.
