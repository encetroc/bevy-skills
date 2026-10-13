---
name: bevy-assets-and-scenes
description: "Implement Bevy asset readiness, shared asset ownership, custom loaders, or scene and animation initialization."
---
# Assets and scenes

## Procedure

1. Resolve Bevy and enabled asset/scene features. Read the matching loading, glTF, or animation example for the changed path.
2. **Define readiness.** Identify all required handles, recursive dependencies, scene instances, and render pipelines. Express readiness as observable states; a created handle alone is not a loaded asset.
3. **Assign asset ownership.** Retain typed handles for the required lifetime. Share immutable meshes/materials by cloning handles; allocate private assets only for independent mutation. Done when a mutation cannot alter unrelated instances.
4. **Separate initialization stages.** Load asynchronously, wait for dependencies, then configure instantiated entities. For animation, attach graphs/transitions to the actual player entity after scene instantiation. Keep loading presentation alive until the required readiness conditions hold.
5. Verify first load, delayed load, failure, and teardown. Exercise the real asset root and target platform; completion requires usable instantiated content and actionable loading errors.

## Branches

- **Custom assets:** register the asset type and loader, keep errors typed, and test malformed input as well as successful loads.
- **Runtime-generated assets:** create through asset storage, reuse handles during updates, and release temporary ownership at teardown.
- **Background work:** compute or load off-thread through supported task pools; return data for ECS-thread application rather than mutating the world from workers.
- **Hot reload:** confirm watcher features and define whether replaced dependencies require scene or animation reinitialization.
- **Platform delivery:** verify filesystem/browser asset paths and feature gates on the actual target, rather than inferring portability from a desktop run.

## Example lookup

Use the target release's `examples/asset/`, `examples/gltf/`, and `examples/animation/`. Consult serialization/reflection examples only when persistence or dynamic inspection is part of the task.
