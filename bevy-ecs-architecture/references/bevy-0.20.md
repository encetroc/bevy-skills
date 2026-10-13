# Bevy 0.20 ECS architecture

## Ordering and observers

- Lifecycle observer bundle parameters move inside the event: `On<Add<MyComponent>>`, not `On<Add, MyComponent>`. Dynamic component observers use patterns such as `On<Add<()>>`. Custom bundle-filtered events need the guide's `EventPattern` migration.
- `chain_weak`, `before_weak`, and `after_weak` preserve ordering only for tracked conflicting access. Several built-in render/extraction/UI sets now use weak ordering. Channels, atomics, globals, or interior mutability through read-only resources need explicit strict edges when correctness depends on sequence. Deferred-effect producers and exclusive systems retain the documented sync/conflict handling.
- With the `debug` feature, `ScheduleBuildSettings::shuffle_seed` can exercise permitted system orderings. Log/replay seeds and combine this with ambiguity detection. Deferred sync placement and greedy multithreaded execution limit what a shuffled build proves.

## State, queries, and errors

- Use `NextState::set_if_different` and `CommandsStatesExt::set_state_if_different` for state requests. `Mut::set_if_neq` remains the distinct component/resource equality guard.
- “Many” query iterators now yield `Result`. Handle missing/nonmatching entities explicitly; use `.matched()` only when intentionally preserving skip behavior. Sorted/parallel variants have different handling described in the guide.
- `#[component(summary_tick)]` opts into per-column change summaries: faster sparse-change queries, more mutation work. Profile before adoption.
- Consider `despawn_all<F: QueryFilter>` for bulk cleanup and verify relationship/teardown behavior. State-scoped `DespawnOnEnter` / `DespawnOnExit` already receive the optimization.
- Panics in systems/commands/observers route to the fallback error handler; the default re-panics. A log-and-continue policy is a deliberate recovery decision, not proof the world remains valid. Attach actionable errors with `ContextExt::context` / `with_context`.

Completion: exercise transitions and cleanup, replay order-sensitive tests, and check custom recovery policies cannot conceal failed initialization.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
