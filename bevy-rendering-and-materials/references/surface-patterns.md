# Procedural surface patterns

Use when logical tile resolution drives excessive terrain or grid geometry.

1. Measure vertices, triangles, and construction cost through the real mesh builder at coarse and dense logical resolutions.
2. Keep tessellation tied to geometric curvature, not logical cell count. Share vertices where the surface permits it; compute pattern detail in a version-matched material extension.
3. Pass logical resolution and toggles through correctly aligned uniforms. Derive pattern coordinates from interpolated position with the same bases, signs, inverse projection, and seam conventions as CPU placement/picking.
4. Filter checkerboard parity over the pixel footprint using derivatives; express grid-line distance in pixel units and fade subpixel lines. Inspect oblique and distant views for aliasing.
5. If shaders are embedded, verify the asset URI uses the owning crate's namespace, including when source is reused by example crates. Update both registration and references.
6. Toggle the pattern uniform while retaining terrain and placed objects. Test off/on transitions and dense scenes through a real renderer.

Fixed triangles approximate a curved mathematical surface. Exact object contact may require conforming geometry; document or test the approximation.

Completion: geometry remains within the declared budget as logical resolution grows, CPU placement aligns with rendered patterns, and toggles render correctly. Report geometry reduction separately from measured frame-time improvement.
