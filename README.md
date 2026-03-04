# Florida Housing & Migration Analysis (2020–2024)

## Project Overview
This portfolio project analyzes **Florida county-level housing market dynamics** and evaluates whether **net domestic migration** is associated with home value growth.

The housing metric used is Zillow’s **ZHVI (Zillow Home Value Index)**. Migration and population data come from the U.S. Census Bureau’s **Population Estimates Program (CO-EST2024-ALLDATA)**.

This project demonstrates:
- Structured problem framing
- Multi-source data integration
- Panel data transformation (wide → long)
- Feature engineering for time-series analysis
- Regression modeling and interpretation
- Unsupervised clustering
- Geospatial visualization
- Stakeholder-oriented storytelling via Power BI

---

## Business Context
Since 2020, Florida has experienced substantial housing appreciation and heightened affordability concerns. Public narratives frequently attribute price acceleration to domestic in-migration.

This project evaluates county-level housing market dynamics to identify:

1. Geographic concentration of appreciation
2. Post-2022 growth deceleration patterns
3. Statistical association between domestic migration and home value growth

---

## Research Questions

1. **Growth & Cooling:**
   Which Florida counties experienced the strongest ZHVI growth from 2020–2022, and did growth slow after 2022?

2. **Migration Relationship:**
   Is net domestic migration positively associated with county-level YoY home value growth?

3. **Segmentation:**
   Can Florida counties be grouped into distinct housing market profiles based on growth, volatility, and migration?

---

## Hypotheses

- **H1:** Net domestic migration is positively associated with county-level YoY home value growth during 2020–2022.
- **H2:** YoY growth in home values slowed meaningfully after 2022 across most Florida counties.
- **H3:** Florida counties can be clustered into distinct housing market segments (e.g., high-growth/high-migration vs. stable/low-migration).

> This project does not directly test buyer origin (e.g., CA/NY) or cash purchase activity, as those require transaction-level datasets not included in the selected sources.

---

## Data Sources

### 1) Zillow ZHVI
- **Dataset:** County ZHVI (mid-tier, all homes, smoothed, seasonally adjusted)
- **Raw File:** `data/raw/County_zhvi_uc_sfrcondo_tier_0.33_0.67_sm_sa_month.csv`
- **Granularity:** Monthly, county-level

### 2) U.S. Census Population Estimates (PEP)
- **Dataset:** CO-EST2024-ALLDATA (States and Counties), April 1, 2020 – July 1, 2024
- **Raw File:** `data/raw/co-est2024-alldata.csv`
- **Granularity:** Annual, county-level
- **Key Fields:** `SUMLEV`, `STATE`, `COUNTY`, `POPESTIMATE20xx`, `DOMESTICMIG20xx`, `RDOMESTICMIG20xx`

---

## Methodology

### 1. Data Preparation
- Reshape Zillow ZHVI from wide (monthly columns) to long format (county, date, zhvi)
- Filter to Florida counties
- Convert monthly ZHVI to annual values (annual averages)
- Engineer features:
  - YoY growth
  - Cumulative growth (2020–2022)
  - Post-2022 growth change
  - Volatility (standard deviation of YoY growth)

### 2. Census Processing
- Filter Census to `SUMLEV == 050` (county-level) and Florida
- Construct standardized 5-digit FIPS codes
- Reshape annual migration and population columns from wide to long
- Build clean county-year panel dataset

### 3. Merge
- Merge Zillow and Census datasets on county FIPS + year
- Validate match rate and inspect missing joins

### 4. Analysis
- Exploratory data analysis (distributions, correlations, rankings)
- Linear regression (YoY growth ~ domestic migration [+ optional controls])
- K-means clustering (with elbow method validation)
- Geospatial choropleths for growth, migration, and clusters

### 5. Feature Engineering

Engineered modeling-ready features:

- **YoY Growth** — Annual percent change in ZHVI
- **Boom Intensity (2020–2022)** — Cumulative growth during surge period
- **Cooling Intensity (2024 vs 2022)** — Growth deceleration metric
- **Volatility** — Standard deviation of YoY growth (2021–2024)

These features allow regression and clustering analysis.

---

## Deliverables

### Exploratory Findings

Preliminary exploratory analysis reveals:

- YoY home value growth was significantly higher during 2021–2022 (boom period) than 2023–2024 (cooling period).
- Net domestic migration shows a moderate positive correlation (~0.24) with YoY growth during the boom period.
- The migration–growth relationship weakens substantially during cooling years.
- Migration rates span similar ranges across periods, but price growth differs materially.

These findings suggest that migration may amplify price growth during expansion phases but has limited explanatory power during market slowdowns.

### Regression Findings

Ordinary Least Squares (OLS) regression was used to evaluate the statistical relationship between net domestic migration and YoY home value growth.

- **Boom Period (2021–2022):**
  - Domestic migration shows a positive and statistically significant association with YoY growth (p &lt; 0.01).
  - The model explains approximately 6% of cross-county variation (R² ≈ 0.06).
  - Interpretation: Migration appears to amplify price growth during expansion phases, though it explains only a modest share of variation.

- **Cooling Period (2023–2024):**
  - The migration coefficient is economically small and statistically insignificant (p &gt; 0.05).
  - Model explanatory power drops substantially (R² ≈ 0.01).
  - Interpretation: Migration does not meaningfully explain cross-county growth differences during market slowdowns.

| Period | Migration Coefficient | p-value | R² |
|--------|----------------------|---------|-----|
| Boom (2021–2022) | ~0.0005 | 0.005 | 0.059 |
| Cooling (2023–2024) | ~-0.0002 | 0.235 | 0.011 |

These results support a regime-dependent relationship between migration and housing price growth.

### Clustering Findings

K‑means clustering was used to segment Florida counties into housing market profiles based on four engineered features:

- Boom growth (2020–2022)
- Cooling change (2024 vs 2022)
- Growth volatility
- Net domestic migration rate

The elbow method suggested **three clusters (k = 3)** as a reasonable balance between model simplicity and explanatory power.

Cluster averages show three distinct market segments:

| Cluster | Boom Growth | Migration Rate | Volatility | Cooling Change | Interpretation |
|--------|-------------|---------------|------------|---------------|---------------|
| 0 | Lower | Low | Low | Mild | **Stable markets** – counties with relatively low migration and more stable housing growth |
| 1 | Highest | Very High | Highest | Strongest slowdown | **Migration boom markets** – counties with the largest migration inflows and strongest housing appreciation during the boom period |
| 2 | Moderate | Moderate | Moderate | Moderate slowdown | **Balanced growth markets** – large metro and suburban counties with steady migration and growth |

Visualization of migration vs. boom growth shows clear separation between clusters, with high‑migration counties concentrated in the highest‑growth segment.

These results support the hypothesis that Florida housing markets can be segmented into distinct profiles based on migration pressure and growth dynamics.

---

## Skills Demonstrated

- Data reshaping (wide-to-long panel transformations)
- Multi-source integration via FIPS join keys
- Time-series feature engineering
- Linear regression modeling (statsmodels)
- Unsupervised learning (k-means clustering)
- Geospatial visualization (GeoPandas)
- Dashboard storytelling (Power BI)

---

## Repository Structure

```
florida-housing-migration-analysis/
├── README.md
├── REFERENCE.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── raw/
│   │   ├── co-est2024-alldata.csv
│   │   └── County_zhvi_uc_sfrcondo_tier_0.33_0.67_sm_sa_month.csv
│   └── processed/
│       ├── zhvi_fl_long.csv
│       ├── census_fl_county_year.csv
│       ├── merged_fl_county_year.csv
│       └── merged_fl_county_year_features.csv
│
├── notebooks/
│   ├── 01_data_loading_zhvi.ipynb
│   ├── 02_data_loading_census.ipynb
│   ├── 03_merging.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_exploratory_analysis.ipynb
│   ├── 06_regression_analysis.ipynb
│   ├── 07_clustering.ipynb
│
├── visuals/
│   ├── domestic_migration_distribution.png
│   ├── migration_vs_growth_all.png
│   ├── migration_vs_growth_boom.png
│   ├── migration_vs_growth_cooling.png
│   ├── migration_vs_growth_colored.png
│   ├── yoy_growth_distribution.png
│   ├── housing_clusters_scatter.png
│   ├── housing_clusters_stability.png
│
└── dashboard/ (Power BI dashboard files)
```

---

## Setup

```bash
pip install -r requirements.txt
```

Core dependencies include:
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- statsmodels
- geopandas
- shapely

---

## Project Status

- [x] Repository scaffolded
- [x] Raw datasets integrated
- [x] Zillow reshaped to panel format
- [x] Census reshaped to panel format
- [x] Merge Zillow and Census datasets
- [x] Annualize ZHVI + feature engineering
- [x] Exploratory data analysis (EDA)
- [x] Regression modeling
- [x] Clustering
- [ ] Geospatial mapping
- [ ] Power BI dashboard

---

## Notes for Recruiters

This project emphasizes:

- Clear problem framing
- Reproducible Python workflows
- Structured multi-dataset integration
- Defensible statistical reasoning
- Transparent discussion of limitations
- Stakeholder-oriented storytelling through an interactive Power BI dashboard summarizing housing market segments and migration dynamics.
