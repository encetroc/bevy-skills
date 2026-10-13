# Camera verification scenarios

Use actual camera-projected rays and viewport coordinates instead of near-target ray fixtures that bypass framing or occlusion.

For focus/orbit cameras, exercise entry → orbit → exit → re-entry at a changed angle. Assert viewport placement, orbit radius/elevation, intermediate transition values, and restored zoom.

For intentionally off-center orthographic framing, check normalized viewport positions at multiple zooms and orbit angles. If the camera model adds a camera-up framing offset, subtract that offset before measuring physical orbit radius/elevation. The offset may be view height × framing fraction × transition blend; derive it from the actual controller.

Assert the full projected object extent and overlay margins, not only its center. Inspect intermediate and final captures for occlusion and readability.
