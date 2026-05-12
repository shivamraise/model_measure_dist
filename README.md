# 3D Measure

A zero-install, single-file browser app for measuring point-to-point distances on 3D STL/STEP models. Drop a file, click to drop waypoints, get the distance.

**Live demo:** https://shivamraise.github.io/model_measure_dist/

No install, no build step — just open `index.html` in a browser, or use the hosted version above.

## Features

- **Drag & drop** `.stl`, `.stp`, or `.step` files to load. STEP face colors (incl. `OVER_RIDING_STYLED_ITEM` overrides) are parsed from the raw text and applied via per-face material groups.
- **Multi-point measurement chains** — click to drop the first waypoint, click to add more, **Esc** to finish. Each chain shows the running total distance in mm in the sidebar table.
- **Straight or spline mode** per chain — toggle between a polyline and a CatmullRom curve. Distance reflects the actual arc length when in spline mode.
- **Edit existing chains:**
  - **Drag a sphere** → snaps to the model surface
  - **Select a sphere → use the X/Y/Z translate gizmo** to move freely along world axes (off-surface motion)
  - **Double-click a segment** → inserts a new waypoint at exactly that point
  - **Select a sphere or line → Delete** to remove
- **Per-measurement color** — click the colored dot in the table for a dark HSV picker with hex input.
- **Global line-thickness slider** — measurements render as 3D `TubeGeometry`, so thickness is real (no WebGL `linewidth=1` constraint).
- **2D outline overlay** — when **Edges** is on, a 2D canvas overlay strokes thin black borders on either side of each colored line + circles around each waypoint, capped tangent to one another. Behaves like a screen-space outline shader without depth artifacts.
- **Selection halos** — selected point/measurement gets an additive white glow so it's distinct from its color.
- **Undo/redo** — `Ctrl+Z` / `Ctrl+Y` (or `Ctrl+Shift+Z`). Tracks: create, add waypoint, drag move, gizmo move, dblclick insert, delete point, delete measurement, color change, spline toggle, clear all. Compound stepping prevents 1-point orphan chains from ever being a visible state.
- **Camera:**
  - Left-drag = orbit around the model center (rotation pivot is fixed at the bbox center, decoupled from pan)
  - Right-drag = pan (pixel-accurate at the depth of the rotation pivot)
  - Scroll = zoom toward cursor (works in both perspective and ortho)
  - Perspective / Ortho toggle, Reset view, Edges toggle in the corner toolbar
- **Snap to nearest vertex** option for precise endpoint placement on faceted models.

## Usage

1. Open the live demo or `index.html` locally in any modern browser (Chrome/Edge recommended).
2. Drop an `.stl` / `.stp` / `.step` file onto the drop zone.
3. Click **+ New measurement**, then click on the model to drop waypoints. **Esc** to finish.
4. Click any sphere or line to select it, drag to move, or use the X/Y/Z gizmo for free-space motion.
5. The sidebar table lists each chain with its color, point count, total distance, and a delete button.

## Tech stack

| Library | Version | Purpose |
|---|---|---|
| [Three.js](https://threejs.org) | r128 | 3D rendering, raycasting, TubeGeometry, TransformControls, STLLoader |
| [occt-import-js](https://github.com/kovacsv/occt-import-js) | 0.0.22 | OpenCascade WASM — STEP parsing & tessellation |

Both are loaded from CDN on first use. After that, the app works offline.

## Browser support

Chrome 89+, Edge 89+, Firefox 90+. STEP files require ~6 MB WASM download on first load (cached afterwards).

## License

MIT
