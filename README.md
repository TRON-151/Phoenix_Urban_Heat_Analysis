# Phoenix Urban Heat Analysis

This repository contains the spatial data analysis of urban heat vulnerability for the City of Phoenix, AZ. The analysis is done by combining Land Surface Temperature (LST), NDVI vegetation data, and census demographics, in order to identity the heat-vulnerable regions.

## About

This project maps **Heat Vulnerability Index (HVI)** across Phoenix's 382 census tracts using summer 2020 satellite imagery from Landsat 8(for LST) and from Sentinel-2(for vegetation index) and ACS demographic data from US Census Bureau. The goal of this analysis is to show environmental justice patterns; where high heat exposure intersects with poverty, elderly populations, and low green space access.

## Why Phoenix

Phoenix is one of the hottest major cities in the US, and it has been studied for over two decades by the Central Arizona-Phoenix Long-Term Ecological Research (CAP LTER) program. That prior research keeps pointing to the same thing: low-income and minority neighborhoods run hotter because they have less vegetation, more paved surfaces, and a history of segregation. So it made a good case study, the data is easy to access, the heat is extreme, and the link between wealth and heat is already well documented (roughly 0.28°C of cooling for every $10,000 of extra income). This project puts numbers to that story for Phoenix specifically.

## Key Findings

The full write-up is in [`Project.Qmd`](Project.Qmd), but here are the main takeaways:

- **Heat is unequal.** The poorest neighborhoods run about **2.2°C hotter (~4°F)** than the wealthiest ones, even after controlling for population density, age structure, and poverty in the regression models.
- **Green space is unequal too.** Wealthier tracts have about **52% more vegetation (NDVI)** than the poorest tracts.
- **Vegetation is the strongest cooler.** Every 1-unit rise in NDVI is linked to roughly a **21°C drop** in surface temperature, the single biggest predictor in the model.
- **The heat clusters, it isn't random.** Global Moran's I, LISA, and Getis-Ord Gi* all confirm hot spots concentrating in the low-income, high-density central core, while cool spots sit in the wealthier periphery.
- **About 20% of residents** live in the highest-vulnerability tracts, the priority areas for cooling centers and tree planting.

## Data

| Dataset | Source | Via | Resolution |
|---|---|---|---|
| LST | Landsat 8 | Google Earth Engine | 30m |
| NDVI | Sentinel-2 | Google Earth Engine | 10m |
| Census Demographics| ACS | US Census Bureau API | Tract level |
| Shape files | City of Phoenix Open Data | Downloadable files | Vector polygons |

## How the Analysis Works

The workflow uses two languages, Python for pulling the raster data and R for everything else. The steps roughly go like this:

1. **Data acquisition** — LST and NDVI are exported from Google Earth Engine (summer 2020, <20% cloud cover), and the census variables come from the US Census Bureau API. All of this is documented in [`data_access.Qmd`](data_access.Qmd).
2. **Prep and zonal stats** — Everything is reprojected to EPSG:2223 (Arizona Central State Plane), then mean LST and NDVI are extracted per census tract using `exactextractr`.
3. **Exploration** — Temperature and vegetation distributions, plus income-quintile breakdowns to see how the richest and poorest tracts compare.
4. **Correlation** — Income vs temperature, income vs vegetation, and the vegetation cooling effect, tied together in a correlation matrix.
5. **Spatial statistics** — Global Moran's I for overall clustering, then LISA and Getis-Ord Gi* to locate the actual hot and cold spots.
6. **Regression** — OLS first, but the residuals were spatially autocorrelated, so spatial lag and spatial error models were fitted. The spatial error model won (AIC dropped from 1129 to 990).
7. **Heat Vulnerability Index** — A composite z-score index combining LST, inverse NDVI, poverty, elderly share, under-18 share, and inverse income into one map.

## Output

**Heat Vulnerability Index (HVI) MAP — Phoenix, Summer 2020**

![HVI Output Map](outputs/hvi_map.png)

> *Composite index combining normalized LST, inverse NDVI, population density, poverty rate, and elderly population share. Higher the values, greater the thermal vulnerability.*

## Repository Structure

```
Phoenix_Urban_Heat_Analysis/
├── Project.Qmd        # Main analysis: stats, maps, regression, HVI
├── data_access.Qmd    # How the LST, NDVI and census data were pulled
├── Data/              # Raster imagery, shape files, census data
├── outputs/           # Generated maps and plots
└── README.md
```

## Reproducing the Analysis

If you want to run this yourself:

1. Start with `data_access.Qmd` to fetch the data. You will need a Google Earth Engine account (for LST/NDVI) and a US Census Bureau API key (for the demographics).
2. Then render `Project.Qmd`, which loads the data from `Data/`, runs the full analysis, and outputs a PDF report with all the maps and tables.

Both files are Quarto documents (`.Qmd`), so you will need Quarto plus R and Python installed.

## Libraries used

`Python` `geemap` `geopandas` 

`R` `sf` `terra` `ggplot2` `tidycensus` `tidyverse`

---
*Analysis by Ousama Bin Zamir < Feburary 2025>* 
