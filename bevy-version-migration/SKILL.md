---
name: bevy-version-migration
description: "Upgrade Bevy dependencies or resolve release-related Rust API, shader, feature, and runtime behavior changes."
---
# Version migration

## Procedure

1. Record resolved source/target versions, dependency compatibility, features, custom shaders, and representative runtime paths. Read official migration guides for every crossed release; match docs/examples to the destination version.
2. Establish a baseline with repository checks and relevant gameplay/render scenarios. Keep the change isolated using the repository's branch/worktree convention and preserve unrelated work.
3. Update dependencies and lockfile. Use compiler diagnostics to migrate Rust APIs, checking all relevant targets and feature profiles. Done when dependency and Rust failures are accounted for.
4. Audit changes that compilation cannot catch: shader language/imports/bindings, embedded asset paths, defaults, scheduling semantics, and platform/backend requirements. Apply only changes required by the target guide or confirmed source.
5. Run tests plus actual renderer-backed paths for changed shaders/materials. Compare baseline behavior and captures; choose explicitly whether to preserve old behavior or adopt new defaults.
6. Review the diff and report migrated boundaries, checks, runtime evidence, and unsupported targets. Completion requires both build success and exercised behavior.

## Version-specific notes

For the old collection's 0.19-to-0.20 observations, read [historical checklist](references/0.19-to-0.20.md) only when crossing that boundary. Treat it as leads to confirm, not a substitute for official guidance.

## Build diagnosis

All-target tests can compile many example harnesses. Inspect build/process progress before diagnosing a deadlock; reduce build parallelism if resource contention is confirmed.
