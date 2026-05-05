# Pulse Topology – Terminal cable viz

**Note:** Developed with AI assistance.

Interactive 3D view of cable hang points from `April23_001.csv` by default (or pass `?data=other.csv`). Each non-gizmo row is a mount at `(position_x, position_y, position_z)`; a vertical cable of length `Bulbcablelength(M)` (or any column matching `*cablelength*`) drops to the bulb. Rows with `AmI_*axis*indicator = True` are **orientation markers only** (designer: origin +X, +Y, +Z) and are drawn as an RGB 3-line cross, not as cable points.

**Scene mapping** (aligned with the Boston `topology-cable-viz` app): `X = mirror(position_x)` about the plan span, `Z = position_y`, and `Y = position_z − zMax` so the highest CSV `position_z` sits at scene **Y = 0** (ceiling plane) and longer drops extend to **more negative Y** — same convention as that app’s `y = −pz` with `pz` as depth below a ceiling at 0.

## Run locally

The app loads `002.csv` with `fetch()`, so you must serve the folder over HTTP (not `file://`).

```bash
cd "/path/to/topology-cable-viz"
python3 -m http.server 8000
```

Open [http://localhost:8000/](http://localhost:8000/) and use `index.html` at the root.

## Controls

- **Color by:** drop length, mount height (z), `lineID`, or `Universe`
- **Point size** / **Drop line opacity** — sliders
- **Mouse:** drag orbit, scroll zoom, right-drag pan, hover points for full CSV row + scene coordinates

## Stack

- [Three.js](https://threejs.org/) (ES modules from CDN)
- No build step; single `index.html`

## Data

- Runtime data: `April23_001.csv` (comma-separated; optional `?data=002.csv` etc.)
- Optional steel catenary curve: Wavefront OBJ with `v` vertices and `l` line segments (default `3D Files/catenaries.obj`). Override with `?catenary=path/to/file.obj`.
- **`3D Files/catenaries.svg`** is regenerated from the OBJ (plan view **X–Z**) via `python3 tools/obj_to_plan_svg.py` — Blender’s Grease-Pencil SVG export was empty.

OBJ vertices use **`px ← vx`, `py ← vz`, `pz ← −vy`** (Blender **Y** negated into CSV-height). By default **`?catLine=auto`** picks the **`lineID`** whose bulbs are **closest** to the bbox-mapped curve (mean nearest-neighbour distance in metres); each candidate tries straight vs **`py`**-mirrored alignment and keeps the better fit. Use **`?catLine=N`** for a fixed row or **`?catLine=rig`** for the full rig bbox. **`?catFlipPy`** applies when `catLine` is not `auto` (auto chooses flip per best line). **`?catMap=centroid`** restores centre-shift-only alignment. Same scene mapping as bulbs (mirror X, `Y = z − zMax`, `Z = y`). Large meshes decimated to ~14k vertices.

Simplified from the earlier Boston `topology-cable-viz` static app (no wall-run routing).
