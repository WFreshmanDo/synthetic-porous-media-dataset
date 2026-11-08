# Synthetic Porous Media Image Dataset

**2,000 two-dimensional synthetic porous-media images paired with porosity and permeability labels.**

Author: **Fengrui Wang** · Version: **1.0.0** · Released: **8 November 2026**  
License: **[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**

## Download

- **[Download the complete dataset ZIP](https://github.com/WFreshmanDo/synthetic-porous-media-dataset/releases/download/v1.0.0/synthetic-porous-media-dataset-v1.0.0.zip)**
- [SHA-256 checksum](https://github.com/WFreshmanDo/synthetic-porous-media-dataset/releases/download/v1.0.0/checksums.sha256)
- [Browse the label table](labels.csv)
- [Version 1.0.0 release](https://github.com/WFreshmanDo/synthetic-porous-media-dataset/releases/tag/v1.0.0)

Download and extract the dataset ZIP before running the example below. The
archive includes all 2,000 PNG images, `labels.csv`, this README, the license
and citation metadata. GitHub's automatically generated source-code archives
contain the repository metadata; the complete images are in the dataset ZIP
attached to the release.

## Overview

The dataset provides synthetic binary pore structures and COMSOL-derived
permeability labels for left-to-right flow. It supports pore-structure analysis
and supervised permeability prediction. These are two-dimensional synthetic
structures; they are not measured three-dimensional rock volumes.

| Method | Description | Samples |
|---|---|---:|
| `circle` | Circular solid-particle structures | 300 |
| `voronoi` | Voronoi-derived pore-channel structures | 500 |
| `curved_voronoi` | Curved Voronoi-derived particle structures | 1,200 |
| **Total** | | **2,000** |

Porosity spans **0.15155625-0.4494875**. Permeability spans
**6.5000559e-14-2.4066553e-11 m²**.

## Files in the dataset ZIP

```text
synthetic-porous-media-dataset-v1.0.0/
├── images/          # 2,000 PNG images named by sample_id
├── labels.csv       # One row per image
├── README.md
├── LICENSE          # Full CC BY 4.0 license text
└── CITATION.cff     # Machine-readable citation metadata
```

## Image encoding and physical scale

- Resolution: **400 rows × 400 columns**, unsigned 8-bit grayscale PNG.
- **White (`255`) = pore/fluid; black (`0`) = solid.**
- Physical domain: **1 mm × 1 mm** per sample.
- Pixel spacing: **2.5 μm × 2.5 μm**.
- Porosity is the white-pixel count divided by 160,000.
- PNG pixels preserve the original binary arrays exactly, scaled from 0/1
  to 0/255. Array indexing is `[row, column]`, with no resampling or row flip.
  Use `origin="lower"` when plotting in Cartesian coordinates.

## Label definitions

| Column | Meaning | Unit / values |
|---|---|---|
| `sample_id` | Unique sample identifier and image basename | Text |
| `image_file` | Image path relative to the extracted dataset folder | `images/<sample_id>.png` |
| `method` | Synthetic structure family | `circle`, `voronoi`, `curved_voronoi` |
| `porosity` | Pore-pixel fraction | Dimensionless, between 0 and 1 |
| `permeability_k` | COMSOL-derived left-to-right permeability | m² |
| `log10_permeability_k` | Base-10 logarithm of the numeric permeability value in m² | log10(k / 1 m²) |

Each CSV row matches exactly one image. Labels retain the values and order
of the project's cleaned 2,000-sample table. Paths are relative and the table
contains no machine-specific local paths.

## Label provenance and scope

Permeability labels were obtained from steady-state, two-dimensional
creeping-flow simulations on the corresponding continuous geometries, using
the project's Darcy-law outlet-flow expression. PNG images are rasterized
representations of those geometries; solving flow directly on their pixel
grids may give different numerical permeability values.

This is a minimal image/label release. Continuous geometries, solver models,
pressure/velocity fields, trained networks and the paper's fixed train,
validation and test assignments are outside this release. Exact reproduction
of the paper's model benchmarks additionally requires those assignments and
model configurations.

## Load one sample

Install NumPy and Pillow, then run this example from the extracted dataset
folder, where `labels.csv` and `images/` are located:

```python
import csv
from pathlib import Path

import numpy as np
from PIL import Image

root = Path(".")
with (root / "labels.csv").open(encoding="utf-8", newline="") as stream:
    row = next(csv.DictReader(stream))

with Image.open(root / row["image_file"]) as image:
    pore_mask = (np.asarray(image) == 255).astype(np.uint8)

print(row["sample_id"], pore_mask.shape)
print("Porosity:", pore_mask.mean())
print("Permeability (m^2):", float(row["permeability_k"]))
```

## License and citation

Copyright © 2026 Fengrui Wang. The dataset and accompanying documentation
are licensed under **CC BY 4.0**; see [LICENSE](LICENSE) for the full terms.
Reuse, adaptation and redistribution, including commercial use, are permitted
under those terms. Give appropriate credit, link the license and indicate
whether you made changes.

Suggested citation:

> Wang, Fengrui (2026). Synthetic Porous Media Image Dataset (Version 1.0.0)
> [Data set]. GitHub. https://github.com/WFreshmanDo/synthetic-porous-media-dataset/releases/tag/v1.0.0

Machine-readable metadata are provided in [CITATION.cff](CITATION.cff).
For questions or error reports, open a [GitHub issue](https://github.com/WFreshmanDo/synthetic-porous-media-dataset/issues).

## 中文说明

本数据集由 **Fengrui Wang** 发布，包含 **2,000 张二维合成多孔介质图像**及其
孔隙率、渗透率标签。图像尺寸为 **400×400**，白色为孔隙、黑色为固体；
物理尺寸为 **1 mm×1 mm**，像素间距为 **2.5 μm**，渗透率单位为 **m²**。

请从页面顶部链接下载 Release 中的完整数据集 ZIP，解压后使用。
数据采用 **CC BY 4.0** 许可，使用时应署名、注明许可并说明所作修改。
