
# analysis data

<!-- badges: start -->
<!-- badges: end -->


An R-based spatial analysis pipeline for processing, subsetting, and mapping extreme climate indices (ETCCDI) and historical temperature/precipitation data over Southwest Nigeria.

---

## Overview

- **Region of Interest:** Southwest Nigeria (`Southwest_Nigeria_Region.shp`)
- **Key Indicators Analyzed:**
- **Primary Goals:**
  - Spatial cropping and masking against regional boundaries.

---

## Repository Structure

```text
├── data/
│   └── spatial/             # Shapefiles (Southwest_Nigeria_Region.shp, etc.)
├── scripts/
│   ├── 01_load_and_subset.R # Load NetCDF/raster layers and subset by target years
│   ├── 02_spatial_crop.R    # Align CRS, crop, and mask to Southwest Nigeria
│   └── 03_visualization.R   # Spatial mapping using ggplot2 and tidyterra
├── output/
│   ├── figures/             # Exported spatial maps and plots
│   └── tables/              # Extracted raster values and CSV summaries
├── .gitignore
└── README.md
