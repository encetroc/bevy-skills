# Bevy Skills

Reusable agent skills for Bevy and Avian development. Each skill is in its own directory and follows the Agent Skills format with a `SKILL.md` containing YAML frontmatter (`name` and `description`) and the skill instructions.

## Install

Install the collection:

```bash
npx skills add encetroc/bevy-skills
```

Install just one skill:

```bash
npx skills add encetroc/bevy-skills --skill bevy-best-practices
```

Replace the skill name with any of the names below.

## Included skills

- **anchor-bevy-recentered-floor-texture** — Fix floor textures that appear stationary when the camera and ground mesh are recentered together.
- **bevy-best-practices** — Bevy Rust architecture and implementation guidance, including consulting version-matched official examples.
- **bevy-fontless-direction-chevrons** — Draw font-independent directional chevrons when Bevy UI arrow glyphs render as missing-glyph boxes.
- **bevy-frame-cost-profiling** — Diagnose Bevy frame-rate regressions by separating end-to-end latency from CPU update cost.
- **bevy-grid-relocation-previews** — Implement mouse-driven relocation previews for reserved grid objects in Bevy.
- **bevy-headless-verification** — Develop and verify Bevy games with true windowless rendering, scripted gameplay, and inspected screenshots.
- **bevy-image-target-ui-picking** — Test clickable Bevy UI rendered to an image target in headless playtests.
- **bevy-procedural-surface-patterns** — Decouple dense checkerboards or surface grids from mesh tessellation in Bevy PBR.
- **cube-sphere-projection-consistency** — Change cube-sphere projections while keeping shader patterns, voxel placement, and picking aligned.
- **headless-avian-physics-tests** — Verify Avian 3D rigid-body behavior in Bevy without creating a window.
- **migrate-bevy-0-19-to-0-20** — Migrate Bevy 0.19 applications to 0.20, including API, shader, and rendering changes.

## Contents

Each skill directory contains its `SKILL.md`. Some skills also include supporting reference or example files; keep those files alongside the skill when using or distributing it.
