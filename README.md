# Land Cover Transition Matrix (geoskill-lulc-change-matrix)

> Pixel-wise cross-tabulation of two land cover classification rasters, producing a transition matrix, change area statistics, and Sankey flow data

---

## 1. Overview

Performs pixel-wise cross-tabulation of two land cover classification rasters (integer classes) to quantify transitions between classes. Outputs a transition matrix (rows = earlier epoch t1, columns = later epoch t2, cells = pixel counts), per-class gross loss / gross gain / net change with area statistics, and nodes + links data ready for Sankey flow diagrams. Typical applications: cropland loss / built-up expansion monitoring, land use transition analysis, and national land-change ledgers. Diagonal cells are unchanged pixels; off-diagonal cells are pixels that changed class; combined with a bbox, pixel counts can be converted to square kilometers. Synthetic mode injects a deterministic number of class transitions (e.g. cropland → built_up), so that "detected change = injected change" can be verified offline.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-lulc-change-matrix.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `transition_matrix.csv` | CSV | Transition matrix (with row/column totals) |
| `change_areas.json` | JSON | Per-class gross loss / gain / net change + area in km² |
| `sankey.json` | JSON | Sankey flow nodes + links |
| `change_map.tif` | GeoTIFF (int32) | Change map (0=unchanged, 1=changed) |
| `output-manifest.json` | JSON | Run manifest |


## 6. Technical Principle

(See SKILL.md for details.)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
