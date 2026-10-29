# Fieldspace Studio — offline geodesic field renderer

Extract the ZIP and open `Fieldspace Studio/index.html` directly in a modern browser (Chrome, Firefox, Edge, Safari). **WebGL 2** is needed for shaded scenes. The new geodesic lattice has a canvas fallback even without WebGL 2. No server, CDN, installation or API key is needed. All assets and code are embedded in the HTML.

## Features

- Three editable scalar field components `X[x,y,z]`, `Y[x,y,z]`, `Z[x,y,z]`.
- Wolfram-style expression subset, scalar definitions (`a=2;`) and user-defined functions (`f[x_,y_,z_]:=Sin[x] Exp[-y^2]`). Also honors explicit `X[x_,y_,z_] := ...` in definitions.
- WebGL 2 per-pixel Hamiltonian geodesic tracing for `g = I + α V Vᵀ` with approximate finite-difference gradient, SDF scene intersection, and checker/sphere shading.
- Five reference scenes including **Geodesic lattice** (no spheres, rods, grid or floor); three numerically integrated, color-coded families of lattice geodesics, configurable 3×3 / 5×5 / 7×7 line density and extent. The lattice seeds are deformed according to a metric-inverse-square-root frame, similarly to the local deformation underlying Tissot’s indicatrix.
- Independent, continuous **free-fly/first-person controls:** `W`/`S` forward/backward, `A`/`D` strafe left/right, `Space` ascend, `Shift` descend, `Ctrl` boost movement, `R` reset. Drag to look; use **Capture mouse** to enable pointer lock and `Esc` to release. Adjustable movement speed and optional orbit camera mode.
- Vector arrow overlay, image download, JSON project save/load (including camera and lattice controls), metric and field diagnostics.

## Important scope

In Euclidean positive-definite three-space, **no nonzero vector is null**. This app interprets the user's arbitrary field as a deformation generating the positive-definite metric `g = I + α V Vᵀ` for `α ≥ 0`. The original Euclidean metric is recovered when `V = 0` or `α = 0`. This is a deliberate modeling choice, not a canonical assignment of metric to vector field, nor a Lorentzian metric.

The offline Wolfram-style parser is a subset, **not a full Mathematica kernel**. Supports `Sin`, `Cos`, `Tan`, `Sinh`, `Cosh`, `Tanh`, `ArcSin`, `ArcCos`, `ArcTan`, `Exp`, `Log`, `Sqrt`, `Abs`, `Sign`, `Floor`, `Ceiling`, `Round`, `Min`, `Max`, `Mod`, `Power`, `UnitStep`, `HeavisideTheta`, `If`, `Piecewise`, `Boole`, `Clip`, constants `Pi`, `E`, `Degree`, `GoldenRatio`, power `^`, arithmetic, implicit multiplication, comparisons, logical conjunction/disjunction, simple scalar and fixed-arity function definitions. Symbolic integration, arbitrary patterns, and Mathematica's full definition system require an actual Wolfram Engine/Cloud and cannot run entirely offline in this app.

The lattice traces geodesics of the chosen **positive-definite Riemannian metric**, not **null geodesics**. Real Euclidean/Riemannian 3-space admits no nonconstant null geodesics. Genuine lightlike/null geodesics require an indefinite (e.g. Lorentzian) metric. Curves are integrated independently, so intersections are not guaranteed to form a globally exact geodesic coordinate grid.

Numerical visualization approximates geodesics and is not a certified scientific solver. High-frequency/discontinuous fields and singular functions may produce inaccurate images. Reducing quality/steps improves responsiveness on slower GPUs.

Reference: https://3-dimensional.space/render/ (geodesic rendering and SDF ray marching concept). This is an independently written visualizer, not a copy of that site's implementation.