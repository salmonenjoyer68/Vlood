# Cambodia Flood Risk Prediction

A machine learning project that estimates which villages in Cambodia are most likely to be hit by flooding, using the Cambodia Villages List 2025 combined with open satellite and geospatial data.

## Problem

Cambodia floods almost every year, especially along the Mekong, Tonle Sap, and Bassac. This project frames flood risk as a **supervised classification** problem:

> Given a village's location and environment, was it flooded in a major flood year (e.g. 2011, 2013, 2020, 2024)?

The predicted probability is then turned into a risk level (low / medium / high) and shown on a map.

## Data

| Data | Source | Use |
|---|---|---|
| Cambodia Villages List 2025 | NCDD / HDX | One row per village |
| Flood extent maps | Sentinel-1, Dartmouth Flood Observatory, Global Surface Water | Labels (`flooded` = 1 or 0) |
| Elevation and slope | SRTM / Copernicus DEM | Feature |
| Distance to river or lake | OpenStreetMap / HydroSHEDS | Feature |
| Rainfall | CHIRPS | Feature |
| Land cover | ESA WorldCover | Feature |

## Approach

1. Attach coordinates to every village (join on village code, not name)
2. Label each village by overlaying it on historical flood extents
3. Sample elevation, slope, river distance, rainfall, and land cover at each village
4. Split train/test **by province** to avoid spatial leakage between neighboring villages
5. Compare against baselines (predict "no flood", and a simple rule: low elevation + near river)
6. Train logistic regression and random forest / gradient boosting
7. Evaluate with recall and PR-AUC (missing a real flood is worse than a false alarm)
8. Map the results and explain the main risk drivers with feature importance

## Tech Stack

Python, pandas, numpy, scikit-learn, geopandas, rasterio, matplotlib, folium

## Project Structure

```
.
├── data/
│   ├── raw/            # villages list, flood maps, rasters
│   └── processed/      # final feature table
├── notebooks/          # exploration and modeling
├── src/                # data prep and training scripts
├── outputs/            # maps and charts
└── README.md
```

## Status

Work in progress. Currently: collecting data and building the village feature table.

## Limitations

- Labels come from satellite flood maps, so cloud cover and missing years can cause errors
- "Flooded" does not mean "destroyed"; damage data is much rarer and not used yet
- Village points are approximate, so small villages near flood edges may be mislabeled
- This is a risk-mapping tool, not a real-time flood forecast

## Future Work

- Add damage and displacement records
- Add population and building density
- Interactive web map (Streamlit)
- Per-year flood forecasting with rainfall inputs

## License

MIT