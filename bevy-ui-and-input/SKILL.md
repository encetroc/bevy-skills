---
name: bevy-ui-and-input
description: "Implement Bevy UI controls, keyboard or pointer input, hit routing, or image-target UI interaction."
---
# UI and input

## Version gate

Resolve the project's Bevy version/features first. For Bevy 0.20 work or an upgrade to it, read [0.20 task checks](references/bevy-0.20.md) before implementation; retain version-matched APIs for older projects.

## Procedure

1. Resolve the target Bevy version and installed input, picking, UI, and focus plugins. Identify the consumed input seam: raw events, button resources, action abstraction, or picking observers.
2. **Separate layout from behavior.** Give parent containers explicit layout/sizing, style widget states, and make focus/navigation intentional. Read [geometric icons](references/geometric-icons.md) when font coverage makes directional controls unreliable.
3. **Route hits deliberately.** Associate UI roots with the correct camera. Install the backend needed for the target; mark decorative children to ignore picking or explicitly handle propagation. Define which UI interaction owns the pointer so clicks cannot leak into world actions.
4. Preserve held input and one-shot edges across the relevant schedules. Accumulate instantaneous actions for fixed simulation. Done when move/press/hold/release reaches the actual consumer exactly as intended.
5. Test through that input seam and assert the resulting game action, hover/focus state, and absence of click-through. For image-target or scripted tests, read [scripted input](references/scripted-input.md). Inspect rendered controls for readable glyphs, contrast, and correct hit areas.

## Completion

Tests exercise real routing rather than assigning a final interaction state. State which layers were covered: game action, input mapping, picking, or OS delivery. Include keyboard focus and disabled behavior when those controls support them.
