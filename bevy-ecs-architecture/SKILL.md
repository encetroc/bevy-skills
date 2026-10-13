---
name: bevy-ecs-architecture
description: "Design Bevy plugins, ECS data ownership, queries, system ordering, or state and fixed-step lifecycles."
---
# ECS architecture

## Procedure

1. **Locate the boundary.** Resolve Bevy and features from the manifest and lockfile. Read the matching official example for the ECS seam being changed. Identify the feature's public configuration, mutable state, and callers before choosing APIs.
2. **Assign ownership.** Keep entity state in components, shared state in resources, and system-private bookkeeping in `Local`. Group one coherent feature's registration and lifecycle in a configurable plugin. Done when every mutable value has one owner and the feature can be composed without unrelated setup edits.
3. **Choose delivery semantics.** Use buffered messages for scheduled communication and triggered events/observers for reactive behavior, using the target release's APIs. Specify whether a response must happen in the same frame; register every communication type and order its consumers accordingly.
4. **Make dependencies explicit.** Name system sets at feature boundaries; constrain only real data dependencies. Account for deferred commands, transform propagation, input processing, and state transitions. Done when correctness survives arbitrary ordering of unconstrained systems.
5. **Prove lifecycle behavior.** Test startup, ordinary updates, transitions, and teardown through the feature plugin. Check query cardinality during loading/cleanup and run the repository's checks. Completion requires observed state changes, not merely successful registration.

## Design rules

- Prefer precise system parameters and deferred `Commands`; use exclusive world access for operations that genuinely require immediate arbitrary access.
- Express disjoint mutable access with query filters or `ParamSet`. Use singleton parameters only when the lifecycle guarantees cardinality; represent temporary absence explicitly.
- Use relationships for hierarchy rather than maintaining duplicate forward/reverse links. Use required components or constructors for entity invariants.
- Mutable dereference can mark a value changed even when it stays equal. Read before writing and use semantic equality guards such as `set_if_neq` where supported.
- Put simulation requiring fixed steps in `FixedUpdate`; accumulate frame input before it and interpolate presentation afterwards. Distinguish real, virtual, and fixed time.
- Use enter/exit schedules for state lifecycle and run conditions for ongoing work. Scope transient entities to their owning state; model orthogonal states without enumerating every combination.
- Return worker-task results through a queue/channel and apply them on the ECS thread. Reflection and remote inspection are opt-in interfaces: register the type data their consumers require.

## Example lookup

In the matching Bevy source tree, start with `examples/ecs/`, `examples/state/`, `examples/app/`, and the fixed-timestep movement example. Treat example names and API spellings as release-specific.
