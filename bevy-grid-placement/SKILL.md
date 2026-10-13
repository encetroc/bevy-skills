---
name: bevy-grid-placement
description: "Implement Bevy grid placement, drag previews, occupancy validation, or atomic move/cancel interactions."
---
# Transactional placement

## Procedure

1. **Define the transaction.** Identify the original footprint, candidate footprint, validity rule, and commit/cancel outcomes. Keep the original reservation during preview; exclude only its cells when validating self-overlap.
2. Separate ray sampling, hit testing, snapping, and interaction. Sample from current propagated camera transforms; choose the nearest eligible hit with terrain occlusion accounted for. Derive terrain placement independently of object tops and reject behind-origin/off-surface intersections.
3. **Isolate the preview.** Retain original handles and use private materials/meshes for mutable previews. Reuse temporary assets during movement and refresh picking bounds when vertices change. Done when unrelated instances remain unchanged.
4. Derive contact geometry from the same snapped integer footprint used by occupancy. Keep any depth-fighting offset from changing cell-space boundaries. Test actual generated vertices through inverse projection, including seams.
5. Validate before changing reservations. On success replace the old footprint with the new one atomically; on rejection retain the original reservation. Restore original presentation/bounds and release temporary assets on cancel or teardown.
6. Exercise grab, movement, valid/invalid drop, and cancel through real input consumers. Assert occupancy and presentation after each stage; inspect captures for transparent previews, opaque commits, and visible validity feedback.

## Required cases

Own-cell overlap, partial overlap relocation, blocked-drop atomicity, cancellation, off-terrain rays, seams, and picking after mesh movement must each have an observable expected outcome.

A correctly colored patch can be hidden by a blocker. Supply another visible validity cue and test its rendered appearance rather than requiring an occluded patch to be visible.

## Test boundary

A windowless camera-ray seam can test camera conversion and gameplay without OS cursor delivery. Actual raycast tests need the target release's asset/mesh/bounds resources even when graphics are disabled. Document that coverage boundary.
