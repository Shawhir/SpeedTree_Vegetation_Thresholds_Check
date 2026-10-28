# SpeedTree vegetation LOD thresholds check

Copies of the Realistic_Trees GLBs (all 92 species, Spring/Autumn, Adult/Young, Draco/No_Draco/Uncompressed)
with only `ESRI_lod.thresholds` changed. Meshes, textures and materials are untouched.

## Folder names

`ESRI_lod_ssr_L3r1-2_L2r3-4_L1r5-10_ref1620x1080`

| Part | Meaning |
|---|---|
| `ESRI_lod_ssr` | `ESRI_lod.thresholds`, metric `screenSpaceRadius` (CSS px, pixel ratio has no effect) |
| `L3r1-2` | LOD3 (full detail) on visible rows 1 to 2 |
| `L2r3-4` | LOD2 on rows 3 to 4 |
| `L1r5-10` | LOD1 on rows 5 to 10, LOD0 (impostor) after |
| `ref1620x1080` | rows measured in David's explorer view at a 1920 x 1080 window (scene 1620 x 1080) |

Rows are counted from the first visible row at the start of the explorer's orbit, using the camera and
grid spacing the explorer derives from each model's size. Because the metric is screen size, the
switch distance in metres scales with the tree: big trees keep detail further away than shrubs.

How the values are set: screen radius = bounding-sphere radius x f / depth along the view direction,
with f = hypot(view width, height) / 2 / tan(35 deg). This was checked against the engine's LOD
colours. Each model gets the thresholds that put its switches half a row past the last row of each band.

Each folder has `manifest.csv` with the old and new thresholds for every model.
