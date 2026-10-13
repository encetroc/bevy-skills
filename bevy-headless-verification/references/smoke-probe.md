# Infrastructure smoke probe

The isolated crate at `../examples/smoke/` relative to this reference directory pins Bevy 0.19.1. It tests a procedural 2D scene, zero windows, scripted keyboard edges/movement, and two checked PNG saves. It does not cover the game's assets, UI, picking, or gameplay.

Resolve the skill directory from the loaded SKILL.md path; installation locations vary:

```bash
SKILL_DIR=/absolute/path/to/bevy-headless-verification
BUILD_DIR=$(mktemp -d)
RUN_DIR=$(mktemp -d)
CARGO_TARGET_DIR="$BUILD_DIR" cargo build \
  --manifest-path "$SKILL_DIR/examples/smoke/Cargo.toml" --locked
cd "$RUN_DIR"
env -u DISPLAY -u WAYLAND_DISPLAY timeout 120s \
  "$BUILD_DIR/debug/bevy-headless-skill-probe" >run.log 2>&1
```

A compatible target cache may reduce rebuilding. Build success is required before running; the timeout bounds the executable, not compilation.

Require exit zero, the PASS record, and fresh decodable 320×240 `captures/before.png` and `captures/after.png`. Open both: the green square should move from left to right. The probe checks input edges, final x position, visible green pixels, and saved capture count; inspection verifies appearance.

The old collection recorded a successful Linux Mesa llvmpipe/Vulkan run with display variables unset. That is historical evidence for one configuration, not a fresh result or portability guarantee. This probe uses synchronous pipeline compilation and fixed scene warm-up; it cannot establish first-use pipeline or arbitrary asset readiness behavior.
