---
name: bevy-coordinate-mapping
description: "Change Bevy surface projections or coordinate transforms while keeping geometry, shaders, picking, and placement aligned."
---
# Coordinate contracts

## Version gate

Resolve the project's Bevy version/features first. For Bevy 0.20 work or an upgrade to it, read [0.20 task checks](references/bevy-0.20.md) before implementation; retain version-matched APIs for older projects.

## Procedure

1. Inventory every producer/consumer of the mapping: mesh generation, shader coordinates, picking, snapping, footprints, and physical units. Write the coordinate domains, bases, orientation, seam ownership, and forward/inverse contract.
2. **Measure the defect.** Use actual generated geometry to measure distortion or alignment. Establish a failing bound rather than selecting a new projection by appearance alone.
3. Centralize CPU forward/inverse transforms and route all consumers through them. Mirror the contract in shaders and test representative CPU/shader coordinates. For cube-sphere changes, read [cube-sphere mapping](references/cube-sphere.md).
4. Recompute physical units and fixtures derived from the previous projection. Done when cell spacing, heights, contact assumptions, and boundary ownership have explicit definitions.
5. Test round trips across radii/scales, face edges/corners, out-of-domain handling, and seam continuity. Exercise picking and placement on the actual rendered surface.
6. Inspect ordinary and seam-crossing captures; completion requires numerical invariants and visible footprints/hits to agree.

## Contract rules

- Distinguish mathematical surfaces from tessellated approximations; contacts may require conforming geometry.
- Rendering-only changes leave gameplay coordinates stale. Account for every consumer before declaring the migration complete.
- Keep logical resolution independent from tessellation when only pattern density changes.
