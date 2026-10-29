# Fieldspace Studio v5 — Offline Geometry, Relativity & Voxel Worlds

**Run:** Extract `Fieldspace_Studio_Offline_v5.zip`, open **`Fieldspace Studio/index.html`** in a modern browser. This version uses **no server, CDN, account, API key, or installed assets**. WebGL 2 enables the GPU view; the voxel scene and geodesic-lattice overlay also have a Canvas 2D fallback (slower on large worlds).

## NEW: Minecraft-like, metric-deformed voxel terrain

In **Metric & integration → Reference geometry**, select **Voxel world · Perlin lattice terrain**. The renderer generates finite, stepped terrain made from cubic cells in a lattice. It uses seeded, 2D gradient Perlin noise with multiple octaves (fractal Brownian motion). The heights are deterministic for each seed, block width, frequency and relief setting. Exposed tops and sides form grassy/dirt/stone blocks, with optional water blocks. You can instead select Desert, Snow, or Alien biomes.

Controls:

- **World seed:** change the reproducible terrain pattern.
- **World width (blocks):** 10, 14, 18, 22, or 26 blocks per side.
- **Lattice spacing / block size:** coordinate spacing between adjacent lattice nodes.
- **Terrain relief** and **Perlin noise frequency:** change the height variation and spatial scale.
- **World biome:** Grassland, Desert, Snow, Alien.
- **Deform vertices with metric geodesics:** turn lattice distortion on or off.
- **Draw cube / lattice edges:** draw visible edges between shared lattice nodes.
- **Add water blocks below sea level:** fill shallow areas with blue voxel cells.
- **Update lattice geometry as time evolves:** optionally rebuild the mesh for current metric time slices. This costs CPU time; disabled by default.
- **Regenerate world**, **View entire world**.

### How lattice alignment works

Each integer grid corner `(i,j,k)` is mapped to **one cached world-space vertex**. Every face that touches that corner uses the identical coordinates, so adjacent cubes have **no cracks or inconsistent corner positions**. With a flat Euclidean metric, cubes form an ordinary axis-aligned Minecraft-like voxel grid. With warping enabled, the map is an approximate *spatial exponential map*: from an origin, use the inverse square-root of the spatial metric to create the initial tangent, then numerically integrate the Riemannian geodesic equation to place each node. Connecting the nodes makes a curved/deformed lattice, and each block's polygonal corners lie exactly on those nodes. The edges between them are rendered as straight segments; **these edges are not individually guaranteed to be geodesics**. This voxel lattice is a **separate geodesic-shot coordinate mesh**, not a precise intersection of the independently integrated geodesic curves in the existing overlay mode. Under strong fields the exponential map can fold, making cells overlap.

**Important physics distinction:** A three-dimensional world of spatial cubes is not a network of 4D spacetime null curves. In 3+1D spacetime mode, the terrain occupies a spatial slice at the chosen coordinate time. The terrain mesh is **rasterized**, not rendered by the 4D null-ray tracer; rays still use null-geodesic integration for the original shaded scenes. Geometry is based on `h_ij = δ_ij + α V_i V_j`, while `T` affects the lapse/clock rates in spacetime mode. There is no gravitational collapse, block collision, voxel editing, or Einstein-equation solver in this version.

## Preserved from v4

- Wolfram-*style* editable `X[x,y,z,t]`, `Y[x,y,z,t]`, `Z[x,y,z,t]`, and `T[x,y,z,t]` components; scalar/user-defined functions and a documented offline **subset** of Wolfram Language. A full Mathematica evaluator requires a Wolfram engine and isn't included.
- Spatial metric `h_ij = δ_ij + α V_i V_j`.
- Optional Lorentzian metric `ds² = −N²dt² + h_ij(dx^i+β^i dt)(dx^j+β^j dt)`, `N = exp(clamp(κT,-6,6))`, `β = σV`, signature (−+++), c = 1. Arbitrary input fields are not assumed to solve Einstein's field equations.
- Relativistic placeable clocks, time evolution, motion-based time dilation and position-dependent clock-rate variation.
- Free-fly first-person WASD controls, Space/Shift vertical movement, mouse capture; Orbit camera; metric-distance movement toggle.
- Old spheres, rods, solids, geodesic-lattice scenes; project JSON save/load and screenshot export.
- Exported project **version 5** preserves world seed, size, relief, frequency, biome, lattice warping, edges, water, and follow-time options. The importer accepts older projects with no terrain settings.

## Tests and limits

Browser tests passed for voxel generation, Perlin seed changes, exact shared node reuse, expected mesh vertex counts, zero-field flatness, nonzero-field spatial warping, biome/world-size controls, previous clock features, project export, and switching scenes. The Canvas fallback rendered a complete world in the testing environment. **A hardware WebGL 2 environment was unavailable**, so the GPU terrain shader's final output was not visually tested. If the GPU path fails, the app attempts the Canvas fallback. For a more responsive experience, lower World width and disable live metric-time rebuilding.

Original reference for geodesic-visualization concepts: https://3-dimensional.space/render/. Code is independently written; not a copy of that site or Minecraft.
## v5.1 shader compilation hotfix

Fixed the WebGL 2 fragment shader error `ERROR: 0:51: 'e' : redefinition`. The 3+1D ray tracer previously declared a 3D ray tangent named `e` and later redeclared `e` as a 2D finite-difference offset in the same function. The normal offset is now named `normalEpsilon`. This affects shader compilation only; field mathematics, terrain generation and project formats are unchanged. The test environment lacks working WebGL 2, so the fix was verified against the source and browser regression checks, not a live GPU compiler.
