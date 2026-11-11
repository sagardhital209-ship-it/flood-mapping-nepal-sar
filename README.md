# Terrain-aware deep learning for rapid SAR flood mapping in the Himalaya

Code for the paper *"Terrain-aware deep learning for rapid SAR flood mapping in the Himalaya: the September 2024 and October 2025 Nepal floods"* (Dhital, Bandhan and Chaulagain, submitted to the *International Journal of Applied Earth Observation and Geoinformation*).

U-Net models are trained on Sentinel-1 SAR (VV, VH), with and without terrain inputs (slope, HAND). They are then tested on two Nepal study areas:
- **Kathmandu Valley**: steep and urban, used as a false-alarm test.
- **Koshi Terai**: a flat floodplain, used for accuracy and exposure analysis.

The study covers two events, September 2024 and October 2025.

## Notebooks

Run the notebooks in Google Colab, in order. Later notebooks read the outputs of earlier ones from Google Drive.

| # | Notebook | What it does | Runtime |
|---|---|---|---|
| 01 | `01_unet_baseline_sen1floods11` | U-Net baseline on Sen1Floods11 (VV, VH) | GPU |
| 02 | `02_terrain_inputs_sar_vs_hand_slope` | Adds slope + HAND from Earth Engine; trains model A (SAR) and B (SAR + terrain), seed 0 | GPU |
| 03 | `03_nepal_sentinel1_data_prep` | Selects Sentinel-1 flood and pre-flood scenes (2024), builds and exports the study-area stacks | CPU |
| 04 | `04_nepal_inference` | Sliding-window inference on Nepal, new-water maps, run time | GPU |
| 05 | `05_multiseed_terrain_gain` | Seeds 1–2 for A and B; mean ± std and paired gain B − A | GPU |
| 06 | `06_s2_validation_koshi` | Independent Sentinel-2 (MNDWI) labels for Koshi; accuracy of all methods | GPU |
| 07 | `07_attention_unet_multiseed` | U-Net with scSE decoder attention (C, D), 3 seeds | GPU |
| 08 | `08_finetune_nepal` | Fine-tuning on an automatically labelled S1/S2 pair (north half), test on south half | GPU |
| 09 | `09_exposure_and_figures` | Exposure (buildings, population, roads) and result figures | GPU |
| 10 | `10_study_area_map` | Study-area map (paper Figure 1) | CPU |
| 11 | `11_event2_scene_check` | Sentinel-1/2 availability for the October 2025 floods | CPU |
| 12 | `12_event_2025_transfer` | Unchanged transfer of all models to the 2025 event; accuracy and exposure | GPU |

`results/baseline_unet_results.csv` holds the notebook 01 baseline scores.

## Setup

1. Open each notebook in Google Colab. Set *Runtime → Change runtime type* to **GPU (T4)** where the table says GPU.
2. Set `PROJECT` in the first cell to your own Google Earth Engine Cloud project ID.
3. Use one Google account for Earth Engine and Drive. Outputs are written to `MyDrive/Paper2_flood/` and `MyDrive/Paper2_flood_nepal/`.

**Notebook 06** also needs a dry-season Sentinel-1 VV/VH scene of the Koshi box on orbit 12. Name it `koshi_terai_dry<date>_orbit12.tif` and put it in `Paper2_flood_nepal/`.

**UNOSAT comparison (optional, notebooks 06 and 09):** download product **FL20240928NPL** (shapefile zip) from UNOSAT and place it in `/content` or `Paper2_flood_nepal/`. The data is not redistributed here.

Python packages: see `requirements.txt` (Colab has most of them preinstalled).

## Data

All inputs are openly available:
- **Sen1Floods11**: Bonafilia et al., 2020.
- **Sentinel-1 GRD and Sentinel-2 L2A**: Copernicus, accessed via Google Earth Engine.
- **Terrain and surface water**: Copernicus DEM GLO-30, MERIT Hydro (HAND), JRC Global Surface Water v1.4.
- **Exposure**: Google Open Buildings v3, WorldPop 2020, OpenStreetMap, FAO GAUL.
- **Basemap**: SRTM 90 m, HydroSHEDS, LSIB.

## Citation

See `CITATION.cff`. Please cite the paper once published.

## Licence

Code: MIT (see `LICENSE`). Third-party data keep their own licences.
