# Visual diagnostics

## Shadows

Compare stationary-scene captures with shadows enabled/disabled after render completion. Isolate object regions and measure shadow-darkened pixels; make the same assertion detect the original defect.

Project the mesh's local bounding-box center through its global transform and camera. Entity translation is not a reliable screen location for world-space-authored vertices.

Tune directional normal bias independently from depth bias: confirm their units in the target release. A normal offset can erase small-object shadows; reducing depth bias may instead introduce terrain acne. Inspect close and wide views.

## Visibility and transient artifacts

Check the components consumed by the renderer, not just an entity's visibility flag. The previous collection recorded that Bevy 0.19.1 retained gizmos required detaching/reinserting the rendering component to hide/show; confirm this in target source before applying.

Capture the action frame and successive frames for flicker, without a settling delay that hides it. Match the application's pipeline-compilation mode. Bind each asynchronous readback to its checkpoint and track all completions.

Use localized masks that include transparent appearance and exclude HUD/terrain. Inspect a failing image to distinguish disappearance from intentional occlusion.
