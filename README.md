# Bevy Skills

Task-oriented agent skills for Bevy and Avian. Each skill has a short workflow with observable completion criteria; specialized techniques live in references loaded only for their branch.

## Install

Install the collection:

```bash
npx skills add encetroc/bevy-skills
```

Or install one skill:

```bash
npx skills add encetroc/bevy-skills --skill bevy-ecs-architecture
```

Keep each skill's references and examples with its `SKILL.md`. Skills are independently installable: local reference links stay inside their own directory.

## Choose a skill

| Task | Skill |
| --- | --- |
| Plugins, state ownership, queries, schedules, state lifecycle | [bevy-ecs-architecture](bevy-ecs-architecture/SKILL.md) |
| Loading barriers, asset sharing, scenes, animation initialization | [bevy-assets-and-scenes](bevy-assets-and-scenes/SKILL.md) |
| Camera composition, materials, procedural patterns, visual defects | [bevy-rendering-and-materials](bevy-rendering-and-materials/SKILL.md) |
| Controls, focus, keyboard/pointer routing, image-target UI | [bevy-ui-and-input](bevy-ui-and-input/SKILL.md) |
| Occupancy, drag previews, atomic drop/cancel | [bevy-grid-placement](bevy-grid-placement/SKILL.md) |
| Surface projections shared by geometry, shaders, and picking | [bevy-coordinate-mapping](bevy-coordinate-mapping/SKILL.md) |
| Deterministic ECS or Avian outcomes without graphics | [bevy-simulation-testing](bevy-simulation-testing/SKILL.md) |
| Real gameplay with windowless rendering and inspected captures | [bevy-headless-verification](bevy-headless-verification/SKILL.md) |
| Frame cost, idle writes, reproducible A/B measurements | [bevy-performance](bevy-performance/SKILL.md) |
| Dependency upgrades and API/shader/behavior compatibility | [bevy-version-migration](bevy-version-migration/SKILL.md) |

These boundaries follow independent tasks, not individual bugs. For example, UI owns hit routing; placement owns the occupancy transaction. Simulation testing proves state/physics; render-headless verification adds visual evidence. A task may use both without loading every skill.

## What changed

The previous collection mixed an all-purpose API survey with single-fix skills. This revision removes the catch-all, gives distinct tasks their own discovery triggers, and retains reusable invariants rather than fixed scene details, machine paths, or mandatory branch conventions.

| Previous skill | Replacement / retained technique |
| --- | --- |
| `bevy-best-practices` | ECS, assets/scenes, rendering, UI/input, simulation testing, and performance workflows |
| `anchor-bevy-recentered-floor-texture` | Rendering → texture anchoring reference |
| `bevy-fontless-direction-chevrons` | UI/input → geometric icons reference |
| `bevy-frame-cost-profiling` | Performance |
| `bevy-grid-relocation-previews` | Grid placement |
| `bevy-headless-verification` | Shorter headless workflow; setup, capture, camera, and smoke references |
| `bevy-image-target-ui-picking` | UI/input → scripted input reference |
| `bevy-procedural-surface-patterns` | Rendering → surface patterns reference |
| `cube-sphere-projection-consistency` | Coordinate mapping → cube-sphere reference |
| `headless-avian-physics-tests` | Simulation testing → Avian reference |
| `migrate-bevy-0-19-to-0-20` | Version migration → historical release checklist |

Old names are replaced, not aliases; update explicit invocations and reinstall selected skills under their new names. Remove obsolete installed copies through your skill manager to avoid duplicate discovery.

## Version and evidence boundaries

Resolve APIs from the target project's manifest/lockfile and matched official source/examples. Version-specific references are starting points to confirm, not universal API promises. The 0.19-to-0.20 checklist is explicitly historical and has not been revalidated in this revision.

The unchanged headless smoke crate pins Bevy 0.19.1. It verifies infrastructure, not an arbitrary game. Follow its [run recipe](bevy-headless-verification/references/smoke-probe.md) when diagnosing the environment.
