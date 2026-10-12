---
name: "bevy-fontless-direction-chevrons"
description: "Fix Bevy UI arrows rendering as missing-glyph boxes by drawing font-independent directional chevrons."
version: 1
created: "2026-10-08"
updated: "2026-10-08"
---
## When to Use
Directional arrow Text renders as boxes in Bevy screenshots and a custom font is unnecessary.

## Procedure
1. Inspect the target version's examples/ui/ui_transform.rs.
2. Draw a 12px square Node with only top/right borders of 2px and a BorderColor. Set Pickable::IGNORE on this decorative child of the clickable edge.
3. Rotate with UiTransform::from_rotation(Rot2::radians(atan2(dy,dx)+PI/4)) for screen direction (dx,dy), where positive y points down.
4. Preserve parent hit areas and verify real rendered cardinal/diagonal orientations and pointer travel.

## Pitfalls
- Default fonts may lack Unicode arrows; Text assertions alone cannot prove rendering.
- Decorative children must ignore picking to preserve parent handlers.

## Verification
1. Inspect screenshots for correctly oriented chevrons rather than missing-glyph boxes.
2. Exercise real parent pointer move/press/release and assert travel works.