# UAV Microplot Extraction & Vegetation Index Pipeline

This repository provides a complete Python/Jupyter workflow to:

Crop and rotate microplots from RGB and multispectral (MSP) orthomosaics using plot polygons from a GeoJSON.

Generate stacked RGB+MSP microplot data cubes.

Compute vegetation indices (NDVI, NDRE, GNDVI, SAVI, MSAVI, EVI, etc.).

Calculate summary statistics (mean, median, min, max, std) for each index and each plot.

Export all results to a CSV and produce visualizations.

All functionality is implemented in the provided notebook.

📁 Directory Structure

Expected input directory:

PBM_1.7.2_datasetsM172_RGB_MS/
├── UAV_2306_RGB_barley_plots.tif        # RGB orthomosaic
├── UAV_2306_MSP_barley_plots.tif        # Multispectral orthomosaic
└── plots_final.geojson                  # GeoJSON with microplot polygons


The notebook creates:

PBM_1.7.2_datasetsM172_RGB_MS/
└── microplots_output/
    ├── rgb/                             # Rotated RGB microplots (.npy)
    ├── msp/                             # Rotated MSP microplots (.npy)
    ├── stack/                           # Stacked RGB+MSP cubes (.npy)
    └── indices/
        ├── optional index maps          # (*.npy)
        └── microplots_indices_summary.csv

🛠 Installation
Conda (recommended)
conda create -n microplots python=3.11
conda activate microplots

conda install -c conda-forge \
  numpy pandas geopandas rasterio shapely scipy scikit-image tqdm matplotlib

Pip
pip install numpy pandas geopandas rasterio shapely scipy scikit-image tqdm matplotlib

⚙️ Workflow Overview

The notebook is divided into three major sections:

1. Microplot Cropping, Polygon Processing & Stack Creation

This step:

Loads the orthomosaics and plot polygons.

Reprojects polygons to raster CRS.

Extracts central area of each plot (optional).

Computes polygon orientation.

Crops & rotates RGB and MSP microplots.

Resamples one modality to match the other.

Creates stacked cubes with the following band order:

0: R   (RGB)
1: G   (RGB)
2: B   (RGB)
3: Blue     (MSP)
4: Green    (MSP)
5: Red      (MSP)
6: Red Edge (MSP)
7: NIR      (MSP)


Key configuration in the notebook:

SAVE_RGB = True
SAVE_MSP = True
SAVE_STACK = True

STACK_REF = "msp"         # "rgb" or "msp"
PLOT_ID_FIELD = "id"
CENTER_PERCENT = 60.0     # crop central area of polygon

2. Vegetation Index Computation & Summary CSV

Each stacked microplot (*_stack.npy) is used to compute:

NDVI

NDRE

GNDVI

SAVI

OSAVI

MSAVI

EVI, EVI2

NDWI

NDMI

VARI

MCARI

MTVI2

Each index is computed on the 2D raster and then statistics are extracted:

mean
median
min
max
std


These fields are saved to:

microplots_output/indices/microplots_indices_summary.csv


Optional: save each index raster (SAVE_INDEX_MAPS = True).

3. Visualization & Data Exploration

Finally, the notebook:

Loads the summary CSV into a pandas DataFrame.

Automatically detects available indices.

Generates plots of microplot statistics.

Provides helper tools to inspect per-index distributions.

Main visualization function:

plot_index_stats(df)

🔧 Customization
Polygon area extraction

Adjust how much of each plot is used:

CENTER_PERCENT = 50.0

Change the plot naming

Use an attribute from the GeoJSON:

PLOT_ID_FIELD = "plot_number"

Modify vegetation indices

Add your own function:

def idx_custom(b):
    return (b["nir"] - b["green"]) / (b["nir"] + b["green"])

INDEX_FUNCS["custom"] = idx_custom

Change multispectral band mapping

Match your sensor:

MSP_BAND_INDEX = {
    "blue": 0,
    "green": 1,
    "red": 2,
    "red_edge": 3,
    "nir": 4,
}

⚠️ Notes

GeoJSON polygons must overlap orthomosaics.

GeoJSON CRS is automatically reprojected to raster CRS.

Invalid/padding pixels are removed with a simple mask (sum of key bands > 0).

Rotation uses expand=True: output arrays contain only valid rotated content.

📄 License

Specify your license here.
