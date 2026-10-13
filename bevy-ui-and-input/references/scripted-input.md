# Scripted input and image-target picking

## Keyboard and action resources

For resource-based `ButtonInput` injection, run after the input stage clears transient edges and before game consumers. Raw keyboard messages instead arrive before input processing. Confirm the named system set in the target release.

Press once, retain held state over ticks, release once. Clearing edge flags does not release a held button; explicitly release/reset before another click. One render update need not equal one fixed tick.

For an action plugin, use its supported seam and disclose whether mapping is covered. Direct game commands test action handlers, not keyboard/pointer mapping.

## Image-target pointers

Follow the matching official `examples/ui/render_ui_to_texture.rs`:

1. Attach UI to the actual image-target camera with the release's UI target component.
2. Create a pointer identity and location referencing the normalized image target.
3. Send pointer move/press/release messages with coordinates in the target's logical/physical scale and viewport. Schedule in the picking input stage before hit testing and consumers; use separate ticks for required edges.
4. If world gameplay also consumes cursor/button resources, feed them from the same coordinates/timing. Replace window-only viewport assumptions with an explicit provider.
5. Assert the action and that the same press produced no unintended ground action.

Mouse button state alone has no cursor position. Assigning a final pressed/interaction state bypasses picking.

For Bevy 0.20, read [UI/input migration checks](bevy-0.20.md): pointer observers use flat events (for example `On<PointerPress>`), metadata moves to `pointer.id` / `pointer.position`, and `pointer.location()` supplies target metadata. Legacy `Interaction` is deprecated; use the current widget/picking path.

The prior skill recorded that Bevy 0.19.1 legacy `Interaction` handled window targets rather than image targets. Check `bevy_ui/src/focus.rs` and use supported pointer observers for off-screen controls; this is a version-specific compatibility note.

For journeys with movement before a screen transition, start away from the requested exit. Assert the intermediate destination before the final transition, so a correct immediate transition is not mistaken for a defect.
