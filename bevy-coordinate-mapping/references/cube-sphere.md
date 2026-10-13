# Cube-sphere projection changes

For equal-angle coordinates u in [-1, 1], the plane coordinate is tan(pi*u/4). Invert with 4*atan(w)/pi after dividing the direction's in-face components by its face-normal component. Apply the declared face basis/sign conventions consistently.

Validate formulas against executable reference code, not explanatory prose alone. The previous collection recorded conflicting prose in a Red Blob appendix; preserve formula tests rather than depending on that page's wording.

Measure tile area by summing spherical triangle areas from actual grid directions. Equal-angle reduces area variation but does not preserve equal area, aspect ratio, or right angles everywhere. The prior 64-cell fixture reported largest/smallest area ratios near 1.4 for equal-angle and 5.0 for normalized-linear mapping; use these as sanity checks, not universal acceptance bounds.

A face-center arc spans pi*radius/2. If nominal cell/height units use that convention, recompute them after switching projection. Update fixtures that embed the old inverse or physical heights.

Cover terrain vertices, voxel faces, support picking, snapping, and anchor selection. Verify seams and forward/inverse round trips at multiple radii, then check rendered placement footprints and grab/drop validity.
