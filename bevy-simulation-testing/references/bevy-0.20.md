# Bevy 0.20 simulation tests

## Reproducible ordering tests

With Bevy's `debug` feature enabled, set `ScheduleBuildSettings::shuffle_seed: Some(seed)` on the tested schedule. Run multiple recorded seeds and replay failures; retain a fixed seed for each regression. Randomized schedule construction complements ambiguity detection rather than replacing it.

Explicit ordering is respected. Automatic deferred sync placement can accidentally preserve a missing dependency, and the greedy multithreaded executor can execute a different order than the shuffled list. Use the single-threaded executor when isolating a build-order failure, then also exercise the production executor.

Weak `chain_weak` / `before_weak` / `after_weak` ordering does not cover hidden read-only interior mutability, channels, or globals. Test those seams and add strict edges where necessary.

## Lifecycle and failure checks

- Update lifecycle observer signatures to `On<Add<T>>` and state requests to `NextState::set_if_different`.
- Many-entity query iterators yield `Result`; missing entity failures should be observable unless intentional filtering with `.matched()` is the test's contract.
- The fallback error handler now receives caught system/command/observer panics. The default re-panics; a custom log-and-continue handler must not turn a failed regression into a pass. Assert the relevant error/failure state.
- BSN hierarchy-dependent tests can observe `Ready`, but simulation-only readiness does not imply GPU pipeline readiness.
- Resolve Avian's declared Bevy compatibility and current prerequisites before upgrading physics fixtures. Neither the release announcement nor an inherited 0.19 plugin list proves compatibility.

Completion: deterministic fixture results, replayable shuffled-order failures, and teardown/error assertions with renderer and OS coverage excluded.

## Sources and evidence

Reviewed against the official [Bevy 0.20 release notes](https://bevy.org/news/bevy-0-20/) and [0.19 → 0.20 migration guide](https://bevy.org/learn/migration-guides/0-19-to-0-20/). These are documentation-backed checks, not runtime validation. Resolve exact APIs/features from the project's lockfile and version-matched source; preserve older-version behavior when the project has not upgraded.
