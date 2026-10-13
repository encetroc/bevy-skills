# Bevy 0.20 rendering and materials

## Shader and material seams

- naga_oil preprocessing is removed. Translate custom preprocessed shaders to WESL and rename `.wgsl` to `.wesl`; plain directive-free WGSL remains supported.
- WESL imports come first, end in semicolons, and use module paths matching shader paths (for example `bevy_pbr::render::forward_io::VertexOutput`). Replace `#ifdef` with declaration/statement-attached `@if`, and substituted definitions with `constants::NAME`.
- `#define_import_path` is removed. Embedded shaders import at crate/file paths; update registration and every URI together. WESL is always enabled; `shader_format_wesl` and GLSL support are removed.
- Use `MaterialExtension2d` with `ExtendedMaterial2d` for 2D mesh extensions; custom sprite extensions use the new `SpriteMaterial` component and `SpriteMaterialPlugin`. Follow the release's `sprite_material` example. The old internal types with those names were renamed `SpriteMeshMaterial` / `SpriteMeshMaterialPlugin`; distinguish the two APIs.
- Unified sprite rendering can change ordering at equal Z. Assign intentional Z levels for overlapping sprites. `Sprite::alpha_mode` defaults to blend.
- `UnpreparedBindGroup` becomes `BindGroupBuilder`; typed `ShaderBuffer` storage and explicit GPU size also change. Check alignment and sizing from the guide for custom bindings.

## Cameras, defaults, and backends

- Add `ScreenSpaceTransmission` to `Camera3d` only when screen-space specular transmission is required; it is no longer required/enabled by default.
- `Tonemapping` and `DebandDither` moved to `bevy_render::view`. `Tonemapping::None` is now full passthrough, so recheck color management.
- Stacked cameras share a resolved compositing space; conflicting requests fall back to linear with a warning. Mixed-HDR stacks now auto-composite upper-camera blits; use explicit `CameraOutputMode::Write` blend state if old replacement behavior is intended.
- CAD navigation can use `PanOrbitCamera` with `DefaultPanOrbitCameraPlugins` and `MeshPickingPlugin`. Inspect `camera/pan_orbit_camera_cad`; check already-installed plugins before registration and route UI input deliberately.
- Solari supports atmosphere/environment-map lighting and macOS Metal, but macOS has no built-in Solari denoiser. ReSTIR is off by default: inspect many-light/moving shadows and re-enable `SolariLighting::restir` when needed. Point/spot/rect light support is future work in the release notes, not an established 0.20 capability. DLSS users need the updated SDK.
- Mesh shaders have pipeline-cache support for advanced uses, not automatic `StandardMaterial` integration, and are unsupported on web.

## Custom render-world code

Extraction traits now specify the target app (`#[extract_app(RenderApp)]`); infrastructure moved to `bevy_extract`. Built-in render/UI chains use weak ordering, so hidden dependencies need strict edges. UI extraction is retained; inspect changed nodes instead of assuming fresh per-frame data. Window render data is queried as `ExtractedWindow` / `SurfaceData`, and view compositing uses `ResolvedCompositingSpace`.

Completion: load every migrated shader in the real renderer, test overlapping sprites and camera stacks, and compare captures on the supported backends.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
