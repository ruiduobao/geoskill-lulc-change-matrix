---
name: geoskill-lulc-change-matrix
description: '两期土地覆被分类栅格逐像元交叉制表，输出转移矩阵、变化面积统计与 Sankey 流向数据'
---

# 土地覆被转移矩阵 | Land Cover Transition Matrix

Perform pixel-by-pixel cross-tabulation of two land cover classification rasters (integer classes) to quantify transitions between classes. Outputs a transition matrix (rows = earlier epoch t1, columns = later epoch t2, cell values = pixel counts), per-class gross loss / gross gain / net change and area statistics, as well as nodes + links data ready to be used directly in a Sankey flow diagram.

Typical applications: cropland loss / built-up expansion monitoring, land use transition analysis, and national land change ledgers. Diagonal cells are unchanged pixels, while off-diagonal cells are pixels that have transitioned; combined with the bbox, pixel counts can be converted to square kilometers. Synthetic mode injects a known number of class transitions (e.g., cropland → built_up) so that "detected change = injected change" can be verified offline.

## Dependencies / 依赖

```bash
pip install 'numpy' 'rasterio' 'pandas'
```

## Usage / 使用方法

### Basic Usage

```bash
python geoskill-lulc-change-matrix.py --bbox 116.0 39.0 117.0 40.0 --n-classes 5
```

### Example 1 (synthetic data, offline)

```bash
python geoskill-lulc-change-matrix.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2 (read two local classification rasters)

```bash
python geoskill-lulc-change-matrix.py --t1 cls_2015.tif --t2 cls_2020.tif --output-dir ./out
```

### Example 3 (different area + quiet mode)

```bash
python geoskill-lulc-change-matrix.py --bbox 121.0 31.0 122.0 32.0 --synthetic --quiet --output-dir ./out
```

### Example 4 (3-class scenario)

```bash
python geoskill-lulc-change-matrix.py --bbox 116 39 117 40 --synthetic --n-classes 3 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `transition_matrix.csv` | CSV | Transition matrix (with row and column totals) |
| `change_areas.json` | JSON | Per-class gross loss/gain/net change + area in km² |
| `sankey.json` | JSON | Sankey flow nodes + links |
| `change_map.tif` | GeoTIFF (int32) | Change map (0 = unchanged, 1 = changed) |
| `output-manifest.json` | JSON | Run manifest |

## Data Source / 数据源 / Source

- Two local classification GeoTIFFs (integer classes, spatially aligned);
- `--synthetic` generates a two-epoch classification pair offline (no network, no account required).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
