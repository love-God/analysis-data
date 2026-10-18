
# analysis data

<!-- badges: start -->
<!-- badges: end -->

The goal of analysis data is to extract and preprocess extreme climate data for southwest,Nigeria
# Climate Extremes Analysis: Southwest Nigeria

An R-based spatial analysis pipeline for processing, subsetting, and mapping extreme climate indices (ETCCDI) and historical temperature/precipitation data over Southwest Nigeria.

---

## Overview

- **Region of Interest:** Southwest Nigeria (`Southwest_Nigeria_Region.shp`)
- **Key Indicators Analyzed:**
  - **ETCCDI Indices:** `txx` (annual maximum of daily maximum temperature), `tx90p` (warm days), `tn10p` / `tn90p` (cold/warm nights) derived from NetCDF (`.nc`) climate model datasets (e.g., GFDL-CM4, NorESM2-LM, HadGEM3-GC31-LL).
  - **Baseline Climate:** WorldClim historical high-resolution rasters (`tmin`, `tmax`).
- **Primary Goals:**
  - Temporal subsetting of historical NetCDF time-series (e.g., 2000–2014 / 2016).
  - CRS alignment and reprojection to match spatial layers (`EPSG:4326`).
  - Spatial cropping and masking against regional boundaries.
  - Exporting data frames and maps for regional climate impact assessments.

---

## Repository Structure

```text
├── data/
│   ├── raw/                 # NetCDF (.nc) models & WorldClim files (tracked via .gitignore)
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
