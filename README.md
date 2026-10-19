
# analysis data

<!-- badges: start -->
<!-- badges: end -->


An R-based spatial analysis pipeline for processing, subsetting, and mapping extreme climate indices (ETCCDI) and historical temperature/precipitation data over Southwest Nigeria.

---

## Overview

- **Region of Interest:** Southwest Nigeria (`Southwest_Nigeria_Region.shp`)
- **Key Indicators Analyzed:** xx` (annual maximum of daily maximum temperature), `txn` (annual maximum of daily minimum temperature), 'CDD'(Consecutive dry days), 'CWD' (Consecutive Wet days), 'R95' (Very wet days), 'R10mm' (Wet Days frequency) derived from NetCDF (`.nc`) climate model datasets (e.g., GFDL-CM4).
  - **Baseline Climate:** 1961-1990
- **Primary Goals:**
  - Spatial cropping and masking against regional boundaries.
  - Temporal subsetting of historical NetCDF time-series (e.g., 2000–2014,  2026-2040).
  - CRS alignment and reprojection to match spatial layers (`Southwest_Nigeria_Region``EPSG:4326`).
  - Spatial cropping and masking against regional boundaries.
  - Exporting data frames ('nc', 'csv') and maps for regional climate impact assessments.

---

## Repository Structure

```text                      
├── data/                    # NetCDF (.nc) models & CDS CMIP6 Extreme Indices (tracked via .gitignore)
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

#Prerequisites & Installation
This project requires R (>= 4.2) and external libraries for spatial raster/vector processing (GDAL, GEOS, PROJ).

install.packages(c(
  "terra",       # Raster and vector operations
  "sf",          # Simple features spatial handling
  "tidyverse",   # Data manipulation and ggplot2
  "tidyterra",   # ggplot2 extensions for SpatRaster/SpatVector
  "geodata",     # Download boundary and climate rasters directly
  "ncdf4"        # NetCDF file driver
))

#Workflow & Usage

#Load and Temporal Filtering:
#Load the multi-layer NetCDF SpatRaster and extract the desired time window

library(terra)
txx <- rast("data/raw/txxETCCDI_yr_GFDL-CM4_historical.nc")
start_date <- as.Date("2000-01-16")
end_date   <- as.Date("2014-12-16")
txx_sub    <- txx[[time(txx) >= start_date & time(txx) <= end_date]]

#Spatial Crop & Mask:
#Project boundaries and mask the climate layer to Southwest Nigeria:

sw_boundary <- vect("data/spatial/Southwest_Nigeria_Region.shp")
sw_aligned  <- project(sw_boundary, crs(txx_sub))

txx_crop    <- crop(txx_sub, sw_aligned)
txx_sw      <- mask(txx_crop, sw_aligned)

#Plotting & Mapping:
Visualize indices using tidyterra and ggplot2
library(ggplot2)
library(tidyterra)

ggplot() +
  geom_spatraster(data = txx_sw[[1]]) +
  geom_spatvector(data = sw_aligned, fill = NA, color = "black", linewidth = 0.7) +
  scale_fill_viridis_c(name = "°C") +
  theme_minimal() +
  labs(title = "Annual Max Temperature (TXx) - Southwest Nigeria")
  
  
#Data Export  
library(terra)

# Convert the raster to a data frame including longitude (x) and latitude (y)
# na.rm = TRUE drops cells outside the masked boundary

df_southwest_txx <- as.data.frame(txx_sw, xy = TRUE, na.rm = TRUE)
write.csv("df_southwest_txx","C:/Users/MR. FEMI/Desktop.csv")

  
  #Authors & Acknowledgments
Analyst: Femi
Data Providers:
ETCCDI / CMIP6 Climate Indices
 GADM via geodata
 
