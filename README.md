# SpeedTree vegetation LOD thresholds check

Copies of the Realistic_Trees GLBs (all 92 species, Spring/Autumn, Adult/Young, Draco/No_Draco/Uncompressed)
with only `ESRI_lod.thresholds` changed. Meshes, textures and materials are untouched.

## Folders

| Folder | LOD3 | LOD2 | LOD1 | LOD0 | Pinus triangles per frame* |
|---|---|---|---|---|---|
| `ESRI_lod_ssr_L3r1-2_L2r3-4_L1r5-7_ref1620x1080` | rows 1-2 | 3-4 | 5-7 | 8+ | 23.0 M |
| `ESRI_lod_ssr_L3r1-2_L2r3-4_L1r5-10_ref1620x1080` | rows 1-2 | 3-4 | 5-10 | 11+ | 25.0 M |
| `ESRI_lod_ssr_L3r1-4_L2r5-8_L1r9-16_ref1620x1080` | rows 1-4 | 5-8 | 9-16 | 17+ | 41.2 M |

\* Measured on the WebGL draw calls in the explorer view, shadows off. For comparison: current thresholds
170.1 M, legacy trees 76.4 M. Shadows multiply all of these by about 3.

## Folder names

`ESRI_lod_ssr_L3r1-4_L2r5-8_L1r9-16_ref1620x1080`

| Part | Meaning |
|---|---|
| `ESRI_lod_ssr` | `ESRI_lod.thresholds`, metric `screenSpaceRadius` (CSS px, pixel ratio has no effect) |
| `L3r1-4` | LOD3 (full detail) on visible rows 1 to 4 |
| `L2r5-8` | LOD2 on rows 5 to 8 |
| `L1r9-16` | LOD1 on rows 9 to 16, LOD0 (impostor) after |
| `ref1620x1080` | rows measured in David's explorer view at a 1920 x 1080 window (scene 1620 x 1080) |

Rows are counted from the first visible row at the start of the explorer's orbit, using the camera and
grid spacing the explorer derives from each model's size. Because the metric is screen size, the
switch distance in metres scales with the tree: big trees keep detail further away than shrubs.

How the values are set: screen radius = bounding-sphere radius x f / depth along the view direction,
with f = hypot(view width, height) / 2 / tan(35 deg). This was checked against the engine's LOD
colours. Each model gets the thresholds that put its switches half a row past the last row of each band.

## Files

- `<folder>/GLB/<tier>/<Species>/<Species>_<Season>_<Age>.glb`: tiers `Draco`, `No_Draco`, `Uncompressed`
  (342 models each; 13 species have no Young models).
- `<folder>/manifest.csv`: original and new thresholds for every model in that folder.
- `thresholds_all.csv`: every model's original thresholds next to all three variants, one row per model.

Thresholds are `[LOD3, LOD2, LOD1, LOD0]` switch points in screen-space radius (px): a tree uses the
first LOD whose value its on-screen radius reaches.
