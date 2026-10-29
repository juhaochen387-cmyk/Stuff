# Fieldspace Studio v4 — Lorentzian Spacetime & Clock Laboratory

Extract the ZIP and open `Fieldspace Studio/index.html` locally in a modern browser. All runtime code is embedded; no backend, CDN, API key or account is required. WebGL 2 is needed for shaded ray tracing; the spatial geodesic-lattice view has a Canvas 2D fallback.

## Lorentzian 3+1 spacetime

Enable **Simulate 3+1D spacetime metric**. Four user-entered Wolfram-style field components are functions of x, y, z, t:
- V=(X,Y,Z), h_ij = δ_ij + α V_i V_j
- Lapse N=exp(clamp(κ T,-6,6)), with T the fourth field
- Shift β^i = σ V^i
- ds² = -N² dt² + h_ij (dx^i + β^i dt)(dx^j + β^j dt), signature (-+++), with c=1.

Lapse strength κ and shift strength σ are adjustable. The metric can explicitly depend on time. Setting X=Y=Z=T=0 recovers flat Minkowski spacetime. These arbitrary fields **do not automatically solve Einstein's field equations**.

In the shaded WebGL 2 scenes, the spacetime renderer attempts numerical 4D Hamiltonian tracing of null light paths backward in time, using finite-difference derivatives. This path is GPU-intensive. The geodesic **lattice overlay remains a spatial geodesic lattice at a fixed time**, not a null-lattice plot. The 3D spatial-metric mode remains available.

## Place and compare clocks

Add up to 40 clocks with independent x/y/z coordinates, coordinate velocities (dx/dt,dy/dt,dz/dt) and names. Use **Add clock**, **At camera**, or **Click to place** on the coordinate Z-plane, and independently edit each clock in the sidebar. Clock faces and elapsed proper-time values appear in the 3D view.

Enable **Evolve coordinate time** (paused by default). Clocks move with their entered coordinate velocities. Each clock accumulates proper time by midpoint integration of

dτ/dt = sqrt(N² - h_ij (v^i + β^i)(v^j + β^j)).

Non-timelike choices (negative or zero expression inside the square root) are flagged, not assigned imaginary time. Worldlines are prescribed and **not automatically geodesic/free-falling**. Clock comparisons use coordinate-time slices and are not direct observed simultaneity without a synchronization model.

Try X=Y=Z=0 and T=-1/(1+Sqrt[x^2+y^2+z^2]); clocks at (0,0,0) and (4,0,0) then run at different rates when κ>0. In flat spacetime a moving clock with v=0.6 advances at 0.8 proper seconds per coordinate second.

## Existing features

First-person/orbit cameras, WASD + Space/Shift navigation, metric-distance movement toggle, 3D objects, spatial geodesic-lattice overlay, time-dependent scalar equations, JSON project export/import (including clocks and older projects), screenshots, and a subset of Wolfram-style input functions and definitions. This app is **not a complete Mathematica engine**.

## Tested / limitations

Browser tests passed for lapse-based clock differences, flat-spacetime time dilation (v=0.6 -> 0.8), shift contribution, time-dependent fields, non-timelike detection and JSON export. **WebGL 2 was unavailable in the test browser**, so the 4D GPU shader's actual rendering/compilation is unverified. Numerical methods are approximate and are not a validated GR solver. Reference for geodesic visualization concepts: https://3-dimensional.space/render/ (implementation is independent).
