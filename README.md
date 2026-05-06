# Pulse Topology – Terminal cable viz

**Live demo:** [GitHub Pages](https://stephanschulz.github.io/topology-cable-viz-nyc/)

**Note:** Developed with AI assistance.

Interactive 3D view of cable hang points from `April23_001.csv` by default (or pass `?data=other.csv`). Each non-gizmo row is a mount at `(position_x, position_y, position_z)`; a vertical cable of length `Bulbcablelength(M)` (or any column matching `*cablelength*`) drops to the bulb. Rows with `AmI_*axis*indicator = True` are **orientation markers only** (designer: origin +X, +Y, +Z) and are drawn as an RGB 3-line cross, not as cable points.

## Screenshots

### 3D rig

![3D rig view](images/3d-rig.jpg)

### 2D cable unroll

![2D cable unroll](images/2d-unroll.jpg)

**3D coordinates:** The WebGL view uses CSV metres directly with Three.js Y-up: **scene X** = `position_x`, **scene Y** = `position_z`, **scene Z** = `position_y`. Mounts sit at `(px, pz, py)`; cables shorten **scene Y** by `Bulbcablelength(M)` to the bulb. The translucent ceiling plane sits at **scene Y** = max CSV `position_z`.

## Run locally

The app loads the CSV with `fetch()`, so you must serve the folder over HTTP (not `file://`).

```bash
cd "/path/to/topology-cable-viz"
python3 -m http.server 8000
```

Open [http://localhost:8000/](http://localhost:8000/) and use `index.html` at the root.

## Controls

- **Color by:** drop length, mount height (z), `lineID`, or `Universe`
- **Point size** / **Drop line opacity** — sliders
- **Mouse:** drag orbit, scroll zoom, right-drag pan, hover points for full CSV row + CSV positions (mount/bulb)

## Stack

- [Three.js](https://threejs.org/) (ES modules from CDN)
- No build step; single `index.html`

## Data

- Runtime data: `April23_001.csv` (comma-separated; optional `?data=other.csv`).

Simplified from the earlier Boston `topology-cable-viz` static app (no wall-run routing).
