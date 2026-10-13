# Bevy 0.20 assets and scenes

## BSN composition and readiness

- Prefix every scene inclusion with `@`: `@scene_variable`, `@scene_function()`, `@SceneComponent`, or `@{scene_expression}`. Component values and chained builders need no `template_value` wrapper.
- Use `--` between entities in lists and brace-delimited `bsn! {}` / `bsn_list! {}` macro invocations. Enums supporting `Default + Clone` no longer need `VariantDefaults`; specify every variant field.
- Use `On<Ready>` for spawned-scene initialization requiring descendants and initial components. Component `Add` observers run top-down and cannot establish complete hierarchy readiness. Keep GPU pipeline readiness separate; `Ready` does not prove pixels are renderable.
- Distinguish BSN `Ready` from the readiness mechanism used by the project's glTF/legacy scene path; inspect that path's tagged example before replacing its observers.
- The release's “What's Next” section lists .bsn asset authoring and assets-as-entities as future work. A snippet or roadmap mention is not proof of a generally available file loader.

## Texture delivery

`CompressedImageSaver` now uses the `ctt` backend with BCn or ASTC output and generates a full mip chain during processing. This is not automatic mip generation for every runtime-created image.

Choose `compressed_image_saver` for platform-specific output; configure ASTC block size through `BEVY_COMPRESSED_IMAGE_SAVER_ASTC`. Choose `compressed_image_saver_universal` to retain Basis Universal/UASTC behavior and load-time transcoding for cross-platform/WebGPU delivery. Test processed assets on target GPUs rather than assuming a desktop format works everywhere.

Completion: test hierarchy-dependent initialization after delayed dependencies, plus successful and failed processed-texture loads on each supported target.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
