# Bevy 0.20 UI and input

## Routing and controls

- Use flat pointer events such as `On<PointerPress>`, replacing `On<Pointer<Press>>`. Fields now live directly on the event; pointer metadata is `pointer.id` / `pointer.position`. Call `pointer.location()` for target/location metadata.
- Legacy `ui::widgets::Button` and `ui::Interaction` are deprecated. Follow the updated button example using `ui_widgets::Button`, `picking::hover::Hovered`, and `ui::Pressed`; test real picking and focus instead of assigning an interaction state.
- A complete text input now needs both `EditableText` and `TextInput`. `EditableText::viewport` replaces `TextScroll`. Escape releases text focus and propagates: test whether the same key should also cancel a drag or close a parent.
- Headless widget tabs use `TabList`, `Tab`, and `SelectedTab`. `ValueChange<Option<Entity>>` is a selection request, not an automatic update; let the app apply it or attach `tablist_self_update`. Here “headless” means bring-your-own visuals, not windowless execution.
- Prefer available Feathers color input, scrollable/selectable lists, dropdowns, lazy menus, and scrubbable number input over bespoke implementations when appropriate. Lazy menus despawn on close; distinguish that lifecycle from hiding. Number updates use `NumberInputValue`; test hard/soft limits, steps, and precision.
- Custom Feathers themes now map `ThemeToken` to `SemanticToken`, then resolve by `ThemeContext`. Test nested panels/dialogs rather than assuming global colors.

## Layout and scene authoring

- Use `em` relative to the node's `EmSize` and `rem` relative to `RemSize`. `TextFont::default()` now uses `FontSize::Rem(1.)`; test changed root font sizes. `EmSize` propagation is app-owned. `Val`/UI-transform resolution needs both font sizes.
- `FixedNode` positions relative to the target camera viewport and does not inherit the parent's layout, clipping, or transform context. Test overlays and hit areas against that viewport.
- `BorderRadius` fields are `CornerRadius` and resolved fields are `Vec2`; migrate circular values with `CornerRadius::circular` or conversion, and test elliptical corners.
- `CalculatedClip` represents transformed rectangles or `FullyClipped`. Use `contains_point` with physical pixels; fully clipped nodes are neither rendered nor pickable.
- `InlineBox` / `InlineImage` enable content in text flow; custom text readers must account for inline boxes.
- BSN scene inclusions require `@`; component builders need no `template_value`. Separate child entities with `--` and use brace-delimited macros. Use `On<Ready>` when widget setup requires spawned descendants.

Custom UI render code must handle retained extraction and changed-node tracking. Built-in UI set ordering is now weak; add strict edges for dependencies invisible to ECS tracking.

Completion: test pointer/keyboard focus, tabs, text Escape, clipping, font scaling, and lazy-menu teardown on the actual camera target, then inspect rendered controls.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
