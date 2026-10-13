# Font-independent directional icons

Use when missing arrow glyphs render as boxes and adding a font is unnecessary.

Read the release's UI transform example. One chevron construction is a square node with only top/right borders, rotated toward a screen direction. For this border layout, the prior implementation used:

```text
rotation = atan2(dy, dx) + pi / 4
```

Screen y points down. Verify cardinal and diagonal directions against the actual border/rotation conventions. Size the square and border for the design (the original 12 px / 2 px values were fixture choices).

Keep the parent hit area independent of the icon and mark the decorative child to ignore picking. Inspect rendered icons and exercise real pointer travel; text/ECS assertions cannot establish glyph appearance.
