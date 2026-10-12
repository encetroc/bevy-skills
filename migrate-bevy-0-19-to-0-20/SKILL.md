---
name: "migrate-bevy-0-19-to-0-20"
description: "Migrate a Rust Bevy 0.19 app to 0.20, especially compiler failures, custom WGSL-to-WESL shaders, `compute_aabb`/`get_aabb`, and runtime rendering regressions."
version: 1
created: "2026-10-12"
updated: "2026-10-12"
---
## When to Use
Use when upgrading a Bevy 0.19 project to Bevy 0.20 or when the user provides the official 0.19-to-0.20 migration guide.

## Procedure
1. Read the official Bevy 0.19-to-0.20 migration guide and inventory the project's Bevy APIs, custom shaders, render features, and lockfile setup.
2. Create and switch to a dedicated migration branch before editing; update the Bevy dependency and regenerate Cargo.lock.
3. Run `cargo check --all-targets` and use compiler diagnostics to identify Rust API migrations such as `MeshAabb::compute_aabb` to `get_aabb`.
4. Migrate custom naga_oil WGSL shaders to WESL: rename shader files to `.wesl`, translate imports/module paths, `#ifdef` branches and shader-def constants, then update embedded asset references.
5. Review behavior changes that compile cleanly, especially screen-space transmission defaults; explicitly restore prior behavior when the project depends on it.
6. Run formatting, `git diff --check`, `cargo test --all-targets` (limit Cargo build jobs if example test harnesses make compilation appear stalled), and at least one relevant runtime/render smoke check so custom shaders are actually compiled.

## Pitfalls
- A successful Rust compile does not prove runtime WESL shader compilation; exercise a renderer-backed app/example.
- `cargo test --all-targets` compiles many example test harnesses, which can look stalled near the end; inspect processes and retry with `CARGO_BUILD_JOBS=2` rather than assuming a code deadlock.
- Do not mechanically enable ScreenSpaceTransmission unless preserving the application's previous visual behavior warrants it.

## Verification
1. The selected branch is the dedicated migration branch and only intended source/lockfile changes are present.
2. `cargo check --all-targets`, formatting checks, and `cargo test --all-targets` succeed.
3. A renderer-backed smoke check reports successful captures and no shader compilation errors.