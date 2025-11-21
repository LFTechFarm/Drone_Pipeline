# UAV Microplot Extraction & Vegetation Index Pipeline

This repository provides a complete Python/Jupyter workflow to:

1.  **Crop and rotate microplots** from RGB and multispectral (MSP)
    orthomosaics using plot polygons from a GeoJSON.
2.  **Generate stacked RGB+MSP microplot data cubes**.
3.  **Compute vegetation indices** (NDVI, NDRE, GNDVI, SAVI, MSAVI, EVI,
    etc.).
4.  **Calculate summary statistics** (mean, median, min, max, std) for
    each index and each plot.
5.  **Export all results to a CSV** and produce **visualizations**.

All functionality is implemented in the provided notebook.

## Directory Structure

Expected input directory:

    PBM_1.7.2_datasetsM172_RGB_MS/
    ├── UAV_2306_RGB_barley_plots.tif
    ├── UAV_2306_MSP_barley_plots.tif
    └── plots_final.geojson

Outputs:

    microplots_output/
    ├── rgb/
    ├── msp/
    ├── stack/
    └── indices/
        └── microplots_indices_summary.csv

## Installation

### Conda

    conda create -n microplots python=3.11
    conda activate microplots
    conda install -c conda-forge numpy pandas geopandas rasterio shapely scipy scikit-image tqdm matplotlib

### Pip

    pip install numpy pandas geopandas rasterio shapely scipy scikit-image tqdm matplotlib

## Workflow Overview

### 1. Microplot Cropping & Stacking

-   Reprojects and crops polygons.
-   Computes polygon orientation.
-   Crops and rotates RGB/MSP rasters.
-   Builds RGB+MSP stack in the order:

```{=html}
<!-- -->
```
    R, G, B, Blue, Green, Red, Red Edge, NIR

### 2. Vegetation Indices & CSV Export

Computed indices include:

-   NDVI, NDRE, GNDVI, SAVI, OSAVI\
-   MSAVI, EVI, EVI2\
-   NDWI, NDMI\
-   VARI, MCARI, MTVI2

Statistics: mean, median, min, max, std.

Output CSV:

    microplots_indices_summary.csv

### 3. Visualization

Plot index statistics using helper function:

    plot_index_stats(df)

## Customization

-   `CENTER_PERCENT` to adjust polygon inner extraction.
-   `PLOT_ID_FIELD` to control naming.
-   Add/remove vegetation indices.
-   Adjust MSP band mapping.

## License

Add your license here.
