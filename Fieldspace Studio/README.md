# Fieldspace Studio v3 — offline field/metric exploration

Extract the ZIP and open `Fieldspace Studio/index.html` locally in a modern browser. No backend, account, API key, CDN or online assets are required. WebGL 2 is needed for shaded scenes; the geodesic lattice has a Canvas 2D fallback. All runtime code is embedded in the HTML.

## What's new in v3

- **Metric-aware movement checkbox:** When selected, WASD/Space/Shift movement uses the spatial metric `g(x,t) = I + α v(x,t) v(x,t)^T`, with `v=(X,Y,Z)`, to maintain the specified local *proper (metric)* speed. The movement direction is transformed by `g^(-1/2)` (midpoint sampled), so its coordinate distance depends on the local metric; ordinary Euclidean movement is still available. The interface shows cumulative coordinate distance versus proper path length. These are *path lengths*, not shortest geodesic distances between endpoints. Metric-aware movement is opt-in; use the checkbox.
- **Four editable Wolfram-style field components:** `X[x,y,z,t]`, `Y[x,y,z,t]`, `Z[x,y,z,t]`, `T[x,y,z,t]`, with `t` available in every formula. `X[x_,y_,z_,t_] := ...`-style definitions are supported by the existing restricted parser; three-argument spatial definitions are also accepted. `T` is the optional time-evolution rate, not an independent fourth spatial coordinate in the 3D ray shader.
- **Constant time by default:** `t=0`, `T=0`, and the **Evolve time** checkbox is off. The editable time field permits viewing snapshots at chosen times. To see automatic time evolution, set `T` to `1` (or an expression), apply, and enable Evolve time. Time then follows `dt/ds = timeRate * T[x,y,z,t]`, sampled at the camera. The spatial field, WebGL geodesics, field arrows and curved lattice react to the selected time.
- **Explicit camera modes:** Sidebar and viewport controls for First-person and Orbit stay synchronized. WASD strafing, Space/Shift up/down, Ctrl boost, mouse drag, pointer lock, and Reset continue to work. Orbit pans with Shift+drag/right-drag; W/A/S/D also translate the orbit target.
- **Project compatibility:** JSON now includes time settings, fourth component, metric-motion mode, and distance counters. Older three-component projects still import with `T=0`.

## Other features

- Editable Wolfram-style math parser (subset), scalar and user function definitions, nested functions, `If`, `Piecewise`, many standard Wolfram numerical functions; see in-app help.
- WebGL 2 geodesic ray integration of shaded SDF scenes and a separate three-family geodesic lattice mode with configurable line density and extent.
- Export/import project JSON and save PNG.

## Mathematical and compatibility limitations

The spatial metric is positive-definite for `α≥0`: `g_spatial = I₃ + α v vᵀ`. The temporal component `T` controls the simulation's time coordinate and is **not** included in a 4D Lorentzian spacetime metric. This is a time-dependent 3D Riemannian model, **not** a general-relativity simulation, and its spatial geodesics are not true null geodesics. The lattice lines are integrated independently and do not guarantee a global geodesic coordinate grid. Calculations are approximate and do not prove mathematical/physical accuracy.

The offline expression parser is **not a complete Mathematica/Wolfram Language kernel**. It does not implement the full set of symbolic transformations, definitions, patterns, packages or arbitrary Wolfram language programs. No API key is needed for the supported offline functions; full Wolfram evaluation would require a separate engine or service.

Reference (independent implementation): https://3-dimensional.space/render/