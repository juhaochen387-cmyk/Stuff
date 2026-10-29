# Fieldspace Studio v6 — infinite, aligned metric-lattice voxel world

Unzip the archive and open **`Fieldspace Studio/index.html`** directly in a modern browser. It needs no backend, internet connection, external dependency or API key. GPU geodesic shading requires WebGL 2; the voxel world, shared lattice, and rods+spheres have a slower Canvas fallback.

## Infinite voxel world

Select **Voxel world · Perlin lattice terrain**. The world is procedurally generated in deterministic **6×6-block chunks** using the *global* integer x/y voxel coordinates, seeded gradient Perlin noise, and a fixed vertical column range. As you move with WASD, new chunks load around the camera and old chunks unload. There is **no preset horizontal map boundary** and no precomputed finite world. Choose a **1, 2, or 3 chunk render radius** (3×3, 5×5, or 7×7 loaded chunks respectively). Higher values increase CPU/GPU work. The world is effectively unbounded; very large coordinates, highly divergent vector fields, browser memory, and floating-point limits still place practical limits on exploration.

Terrain options: seed, block spacing, relief, Perlin frequency, biome, water, lattice edges, metric deformation, and time-following. Heights are global, so terrain stays deterministic across chunk boundaries and when returning to an area.

## Exact shared vertices between three scene modes

All three **Geodesic lattice**, **3D grid rods + spheres**, and **Voxel world** modes now use one numerical coordinate mapping:

`P(i,j,k) = Exp_origin^h( h(origin)^(-1/2) (spacing * (i,j,k)) )`, numerically approximated at time t for the spatial metric `h = I + α V Vᵀ`.

- A voxel face is built from the very same cached `P(i,j,k)` samples used by the other scenes. Adjacent voxel cubes share exact corner coordinates.
- In **3D grid rods+spheres**, every rod is a line segment between adjacent `P` vertices and each sphere is centered at a `P` vertex.
- In **Geodesic lattice**, the curves are the *same connected polylines* as the rods, but without spheres. They use exactly the same global integer lattice and block spacing. Change the lattice density/extent to show more lines or a larger local window.
- All three modes follow changes to `X`, `Y`, `Z`, metric strength, lattice spacing and (where enabled) coordinate time consistently.

**Mathematical limitation:** This geodesic-exponential chart uses numerical shooting from one origin and straight chord edges between mapped vertices. In curved space, adjacent chord edges are generally **not themselves exact geodesics**. It is not possible to require *all* edges of a globally rectangular grid to be exact geodesics for arbitrary curvature. This implementation prioritizes *exact matching vertices/edges between views*, while approximating the geodesic-coordinate mapping at the nodes. None of these spatial lines are Lorentzian null geodesics. The separate spacetime renderer traces light in four dimensions.

## Other features

User-input `X[x,y,z,t]`, `Y[x,y,z,t]`, `Z[x,y,z,t]`, `T[x,y,z,t]` and limited Mathematica-style definitions; 3+1-dimensional Lorentzian metric mode, positionable proper-time clocks, time dependence, metric-aware movement, first-person and orbit cameras, screenshot/export/import. The Wolfram-like interpreter implements an offline subset, **not full Mathematica**. Arbitrary fields are not automatically solutions to Einstein's field equations.

## Project format and testing

JSON export version **6** stores the streaming radius in the prior `terrain.width` key for backward compatibility. Imports from v5 with world widths of 10–26 are mapped to radius 1. Other existing settings are preserved.

Browser checks covered initial terrain, far-away streaming, chunk eviction, geodesic and rods/spheres geometric identity, project export, and movement. WebGL GPU geodesic/light behavior is separate from the spatial voxel renderer and remains an approximation. For demanding metrics or high render distances, reduce the radius or disable Follow time.

Independent reference for geodesic rendering concepts: https://3-dimensional.space/render/.