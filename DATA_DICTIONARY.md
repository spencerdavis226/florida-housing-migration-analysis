# Data Dictionary — Florida Housing & Migration Analysis (2020–2024)

This document describes the **final analytical dataset** used throughout the project notebooks and modeling workflow.

Final dataset:

`data/processed/merged_fl_county_year_clusters.csv`

Each row represents a **Florida county–year observation**. The dataset was created by:

1. Processing Zillow monthly housing price data
2. Processing U.S. Census population and migration estimates
3. Merging both datasets using county **FIPS code and year**
4. Engineering housing growth features
5. Assigning cluster labels from K-means clustering
6. Joining the final county-level dataset to Census TIGER/Line county boundary shapefiles for geospatial mapping

The dataset contains **335 observations (67 counties × 5 years)** covering **2020–2024**.

---

# 1. Unit of Analysis

The dataset is a **county–year panel dataset**.

Each row represents:

```
county + year
```

Example structure:

| county         | year |
| -------------- | ---- |
| Alachua County | 2020 |
| Alachua County | 2021 |
| Alachua County | 2022 |
| Alachua County | 2023 |
| Alachua County | 2024 |

This panel structure allows the project to analyze:

- housing growth over time
- migration trends across counties
- boom vs. cooling housing market conditions

---

# 2. Core Identifiers

| Column       | Description                                               | Source  |
| ------------ | --------------------------------------------------------- | ------- |
| `fips`       | 5‑digit county FIPS code uniquely identifying each county | Census  |
| `RegionName` | County name from the Zillow dataset                       | Zillow  |
| `STNAME`     | State name field from the Census dataset                  | Census  |
| `CTYNAME`    | County name field from the Census dataset                 | Census  |
| `year`       | Observation year (2020–2024)                              | Derived |

### Merge Method

The Zillow and Census datasets were merged using:

```
fips + year
```

Because the **Zillow dataset served as the left table during merging**, the `RegionName` column is retained as the primary county-name field in the final dataset.

---

# 3. Housing Price Variables

| Column        | Description                                           | Source           |
| ------------- | ----------------------------------------------------- | ---------------- |
| `zhvi_annual` | Annual average Zillow Home Value Index for the county | Zillow / Derived |

### Zillow Home Value Index (ZHVI)

ZHVI measures the **typical home value for a region**.

Dataset characteristics:

- Mid‑tier housing segment
- Includes single‑family homes and condos
- Seasonally adjusted
- Smoothed price index

Original Zillow data is **monthly**. For this project, monthly values were:

1. Filtered to Florida counties
2. Converted to long format
3. Aggregated to **annual county averages**

This annualized value is stored as `zhvi_annual`.

---

# 4. Population and Migration Variables

| Column          | Description                                     | Source                              |
| --------------- | ----------------------------------------------- | ----------------------------------- |
| `population`    | Annual county population estimate               | Census Population Estimates Program |
| `domestic_mig`  | Net domestic migration count                    | Census                              |
| `rdomestic_mig` | Net domestic migration rate per 1,000 residents | Census                              |

### Migration Rate

`rdomestic_mig` represents **net domestic migration per 1,000 residents**.

Interpretation:

- Positive values → net in‑migration
- Negative values → net out‑migration

This variable is the **primary migration metric used in regression and clustering analysis**.

---

# 5. Housing Growth Features

These variables were engineered in the feature engineering notebook to analyze housing market dynamics during and after the pandemic housing boom.

| Column                       | Description                                                    |
| ---------------------------- | -------------------------------------------------------------- |
| `yoy_growth`                 | Year‑over‑year percent change in ZHVI                          |
| `boom_growth_2020_2022`      | Cumulative housing growth during the pandemic boom (2020–2022) |
| `cooling_delta_2024_vs_2022` | Change in YoY growth between the peak boom year and 2024       |
| `yoy_volatility`             | Standard deviation of yearly housing growth for each county    |

### YoY Growth Formula

```
(current_year_ZHVI - previous_year_ZHVI) / previous_year_ZHVI
```

This variable captures **annual housing appreciation or decline**.

### Boom Growth

`boom_growth_2020_2022` measures cumulative price growth during the strongest pandemic housing surge.

### Cooling Delta

`cooling_delta_2024_vs_2022` captures how much growth **slowed after the peak boom year**.

### Volatility

`yoy_volatility` measures **how stable or volatile housing price growth was within each county**.

---

# 6. Clustering Variables

| Column         | Description                                   |
| -------------- | --------------------------------------------- |
| `cluster`      | Numeric cluster label from K‑means clustering |
| `cluster_name` | Human‑readable cluster interpretation         |

Clusters were generated using the following features:

- `boom_growth_2020_2022`
- `cooling_delta_2024_vs_2022`
- `yoy_volatility`
- `rdomestic_mig`

The elbow method supported **k = 3 clusters**, representing different Florida housing market types.

---

# 7. Modeling Variables

The main analytical variables used in regression and clustering include:

| Variable                     | Role in Analysis                                   |
| ---------------------------- | -------------------------------------------------- |
| `yoy_growth`                 | Dependent variable in regression analysis          |
| `rdomestic_mig`              | Independent variable measuring migration intensity |
| `boom_growth_2020_2022`      | Captures pandemic boom housing performance         |
| `cooling_delta_2024_vs_2022` | Measures post‑boom slowdown                        |
| `yoy_volatility`             | Measures housing market stability                  |

These features allow counties to be compared based on:

- housing appreciation
- migration inflows
- volatility of housing markets
- boom vs. cooling performance

---

# 8. Data Sources

### Zillow Research Data

Dataset used:

```
County_zhvi_uc_sfrcondo_tier_0.33_0.67_sm_sa_month.csv
```

Source:

https://www.zillow.com/research/data/

---

### U.S. Census Population Estimates Program

Dataset:

```
CO-EST2024-ALLDATA
```

Source:

https://www.census.gov/programs-surveys/popest.html

This dataset provides:

- annual population estimates
- domestic migration counts
- domestic migration rates

for all U.S. counties.

---

### U.S. Census TIGER/Line County Shapefiles

Dataset used:

```
tl_2023_us_county.shp
```

Related shapefile components used locally include:

- tl_2023_us_county.shp
- tl_2023_us_county.dbf
- tl_2023_us_county.shx
- tl_2023_us_county.prj
- tl_2023_us_county.cpg

Source:

https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html

Use in project:

These files were used in the geospatial analysis notebook to create Florida county choropleth maps by joining county geometries to the processed analytical dataset using county FIPS codes.

Repository note:

The shapefiles are used locally in the workflow but may not be included in the GitHub repository because of file size and multi-file shapefile packaging.
---

# 9. Notes

- All housing values are in **nominal dollars**.
- Migration variables represent **domestic migration only**, not international migration.
- County-level aggregation may hide variation within counties.
- Cluster labels are **interpretive labels assigned after K‑means clustering**.
- Geospatial map outputs depend on local Census TIGER/Line shapefiles even if those files are not stored in the GitHub repository.

---

# 10. Processed Dataset Location

Final modeling dataset:

```
data/processed/merged_fl_county_year_clusters.csv
```

This file serves as the **final analytical dataset used for regression, clustering, and geospatial analysis across the notebooks**.

---

# 11. Intended Use

This data dictionary helps readers:

- understand the structure of the analytical dataset
- interpret the regression and clustering results
- reproduce the workflow in the project notebooks

It is primarily intended for **recruiters, reviewers, and collaborators inspecting the repository**.
