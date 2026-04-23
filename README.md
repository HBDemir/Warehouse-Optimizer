# ⬛ 3D Warehouse Optimizer

A browser-based **3D Bin Packing** tool that lets you define a warehouse and a list of packages, then automatically computes an optimal arrangement using one of three recursive algorithms — all rendered in an interactive Three.js scene.

---

## Features

- **3D interactive viewer** — orbit, zoom, and pan the scene with mouse controls
- **Three packing algorithms** — Guillotine, Branch & Bound, and Extreme Point
- **Fragile package support** — fragile items are always placed on top; no other box can be stacked above them
- **All 6 box rotations** considered during placement
- **Live stats** — placed count, unplaced count, volume utilisation %, total warehouse volume, and elapsed time
- **Hover tooltips** — hover over any box in the 3D view to see its ID, dimensions, position, and volume
- **Preset loader** — one-click sample shipment with 18 packages including fragile items
- **Wireframe toggle** — switch between solid and wireframe rendering
- **Multiple camera views** — Perspective, Top, Front, and Side

---

## How to Use

1. Open `warehouse_optimizer.html` in any modern browser (no server required).
2. **Set warehouse dimensions** — Width (X), Depth (Z), Height (Y) in the left sidebar.
3. **Add packages** — enter an ID, W × D × H dimensions, quantity, and optionally mark as fragile. Click **+ ADD PACKAGE**.
   - Or click **📦 SAMPLE LOAD** to populate a preset list of 18 packages.
4. **Select an algorithm** (see below).
5. Click **▶ RUN OPTIMIZATION**.
6. Explore the result in the 3D viewer. Check the stats panel for efficiency metrics.
7. Click **↺ Reset** to clear everything and start over.

---

## Algorithms

| Algorithm | Description | Speed |
|---|---|---|
| **Guillotine** | Recursively splits the remaining space with guillotine cuts after each placement. Balances fill ratio and low-gravity placement. | Fast |
| **Branch & Bound** | Explores a tree of placement decisions and tracks the globally best solution found so far. Tries up to 4 positions per package per depth level. | Slow (exhaustive) |
| **Extreme Point** | Maintains a set of candidate placement points (corners of placed boxes) and scores each candidate by a gravity + wall-contact model. Derives additional points when needed. | Medium |

All three algorithms respect fragile constraints and try all valid rotations of each box.

---

## Controls

| Action | Control |
|---|---|
| Rotate view | Left-click + drag |
| Zoom | Scroll wheel |
| Pan | Right-click + drag, or Shift + drag |
| Inspect a box | Hover over it |
| Switch camera | Toolbar buttons (PERSP / TOP / FRONT / SIDE) |
| Toggle wireframe | KAFES button in toolbar |

---

## Fragile Packages

- Mark a package as **FRAGILE** using the toggle button before adding it.
- Fragile packages are sorted to the end of the placement queue (placed last, i.e. on top).
- The packer enforces two constraints:
  1. A non-fragile box **cannot** be placed directly on top of a fragile box.
  2. A fragile box **cannot** be placed if another box already rests on top of it.
- Fragile boxes are shown with an orange edge highlight in the 3D view and marked with 🔴 in the legend and tooltip.

---

## Stats Panel

| Metric | Description |
|---|---|
| Placed | Number of packages successfully placed |
| Unplaced | Number of packages that did not fit |
| Fragile (top) | Number of fragile packages placed |
| Volume Utilisation | `(sum of placed box volumes) / (warehouse volume) × 100%` |
| Warehouse Volume | W × D × H in cubic units |
| Time | Algorithm execution time in milliseconds |

---

## Technical Details

- **Runtime:** Pure client-side JavaScript — no build step, no dependencies to install.
- **3D rendering:** [Three.js r128](https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js) via CDN.
- **Fonts:** Space Mono & Syne via Google Fonts.
- **Overlap detection:** Axis-Aligned Bounding Box (AABB) intersection test in 3D.
- **Rotation handling:** All 6 permutations of (W, D, H) are generated and deduplicated before trying placement.

---

## File Structure

```
warehouse_optimizer.html   ← Single self-contained file (HTML + CSS + JS)
```

No backend, no framework, no build toolchain required. Just open the file.
