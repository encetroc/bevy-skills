# Checked capture protocol

Prefer the target release's screenshot API for individual snapshots. Direct `Assets<Image>.data` access does not fetch newly rendered GPU pixels.

For each checkpoint:

1. Record expected state, request tick/frame, target, and unique output path.
2. Request image-target capture (in 0.19.1, `Screenshot::image`).
3. On completion (in 0.19.1, `ScreenshotCaptured`), convert the image and save with checked results. Create parent directories first. A helper that only logs save failure cannot acknowledge success to the runner.
4. Acknowledge only after a successful save; fail conversion/save errors, missing captures, and deadlines.
5. Decode and check dimensions, then inspect landmarks and the checkpoint's visual expectation.

Hold ordinary checkpoint state stable until completion and keep one in-flight request per target. Render/main-world latency means request and callback ticks differ; a fixed number of extra updates is not a completion criterion.

For flicker, capture the action frame and consecutive successors without a settling delay. Multiple in-flight readbacks require per-frame identities and all-complete tracking; verify the renderer's actual capture timing rather than assuming the callback tick identifies the rendered frame.

Use isolated output directories so stale files cannot pass. Pixel hashes vary by adapter, fonts, and settings; use tolerant/localized assertions plus image inspection. Include transparent appearance and expected occlusion in masks.

Missing audio-device warnings mean audio was not verified; they do not by themselves invalidate visual evidence. Preserve actual render/asset/capture errors. Multiple renderer instances may require logging-plugin isolation.
