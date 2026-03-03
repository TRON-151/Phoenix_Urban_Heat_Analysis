# Phoenix Urban Heat Analysis

This repository contains the spatio data analysis of urban heat vulnerability for the City of Phoenix, AZ. The analysis is done by combining Land Surface Temperature (LST), NDVI vegetation data, and census demographics, in order to identity the heat-vulnerable regions.

## About

This project maps **Heat Vulnerability Index (HVI)** across Phoenix's 387 census tracts using summer 2020 satellite imagery from Landsat 8(for LST) and from Sentinel-2(for vegetation index) and ACS demographic data from US Census Bureau. The goal of this analysis is to show environmental justice patterns; where high heat exposure intersects with poverty, elderly populations, and low green space access.

## Data

| Dataset | Source | Via | Resolution |
|---|---|---|---|
| LST | Landsat 8 | Google Earth Engine | 30m |
| NDVI | Sentinel-2 | Google Earth Engine | 10m |
| Census Demographics| ACS | US Census Bureau API | Tract level |
| Shape files | City of Phoenix Open Data | Downloadable files | Vector polygons |

## Output

**Heat Vulnerability Index (HVI) MAP — Phoenix, Summer 2020**

![HVI Output Map](outputs/hvi_map.png)

> *Composite index combining normalized LST, inverse NDVI, population density, poverty rate, and elderly population share. Higher the values, greater the thermal vulnerability.*

## Libraries used

`Python` `geemap` `geopandas` 

`R` `sf` `terra` `ggplot2` `tidycensus` `tidyverse`

---
*Analysis by Ousama Bin Zamir < Feburary 2025>* 
