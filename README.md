

# Florida Housing & Migration Analysis (2020–2026)

## Overview
This repository contains a portfolio project analyzing **Florida county-level home value trends** and their relationship to **net domestic migration**. The core housing metric is Zillow’s **ZHVI (Zillow Home Value Index)** and the migration metric comes from the U.S. Census Bureau’s **Population Estimates Program (CO-EST2024-ALLDATA)**.

The goal is to produce:
- A clean, recruiter-friendly GitHub repo (well-structured notebooks + documentation)
- A Python-based exploratory analysis (EDA)
- A small set of defensible hypotheses tested with regression
- County segmentation via clustering
- A geospatial component (Florida choropleth map)
- A Tableau Public dashboard/storyboard summarizing the narrative

## Business Problem
Housing affordability and rapid price appreciation have been a major concern in Florida since 2020. This project investigates:
1) **Where** home values accelerated the most across Florida counties
2) **When** (and whether) growth slowed after the 2022 rate-hike era
3) Whether **domestic migration** is associated with faster home value growth

## Research Questions
1. **Growth & cooling:** Which Florida counties experienced the strongest ZHVI growth from 2020–2022, and did growth slow after 2022?
2. **Migration relationship:** Is net domestic migration associated with county-level home value growth?
3. **Segmentation:** Can Florida counties be clustered into distinct “market profiles” based on growth, volatility, and migration?

## Hypotheses (Draft)
- **H1:** Counties with higher net domestic migration (absolute or per-capita) experienced higher YoY home value growth during 2020–2022.
- **H2:** YoY growth in home values slowed meaningfully after 2022 across most Florida counties.
- **H3:** Florida counties can be grouped into distinct clusters (e.g., high-growth/high-migration vs. stable/low-migration).

> Note: This project does **not** directly test buyer origin (e.g., CA/NY) or cash purchases, because those require transaction-level data not present in these sources.

## Data Sources
### 1) Zillow ZHVI
- **Dataset:** County ZHVI (mid-tier, all homes, smoothed, seasonally adjusted)
- **File (raw):** `data/raw/County_zhvi_uc_sfrcondo_tier_0.33_0.67_sm_sa_month.csv`
- **Key fields (expected):** county/region identifiers + monthly ZHVI values

### 2) U.S. Census Population Estimates (PEP)
- **Dataset:** CO-EST2024-ALLDATA (States and Counties), April 1, 2020 to July 1, 2024
- **File (raw):** `data/raw/CO-EST2024-ALLDATA.csv` (or equivalent export)
- **Key fields (expected):** `SUMLEV`, `STATE`, `COUNTY`, `STNAME`, `CTYNAME`, `POPESTIMATE20xx`, `DOMESTICMIG20xx` (and related components)

## Methods (Planned)
### Data preparation
- Reshape Zillow ZHVI from wide (monthly columns) to long format (county, date, zhvi)
- Filter to Florida counties
- Convert monthly ZHVI to annual values (annual average or December value—decision documented)
- Create features: YoY growth, cumulative growth (2020–2022), volatility (e.g., std dev of YoY)

### Merge
- Filter Census to `SUMLEV == 050` (county-level) and Florida
- Reshape Census wide-to-long for annual migration/population
- Merge on county + year (prefer FIPS; fallback to cleaned county names)

### Analysis
- EDA: distributions, trends, correlations, county rankings
- Regression: simple, interpretable models (e.g., YoY growth ~ domestic migration [+ optional controls])
- Clustering: k-means (with elbow), interpret clusters with descriptive stats
- Geospatial: Florida choropleth(s) for growth/migration/cluster

## Deliverables
- **Tableau Public dashboard/storyboard:** (link will be added here)
- **GitHub repository:** this repo, including notebooks + README

## Repository Structure
```
florida-housing-migration-analysis/
├── README.md
├── requirements.txt
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_data_loading.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_feature_engineering.ipynb
│   ├── 04_exploratory_analysis.ipynb
│   ├── 05_regression.ipynb
│   ├── 06_clustering.ipynb
│   └── 07_geospatial.ipynb
├── visuals/
└── dashboard/
```

## Setup
1. Create a virtual environment
2. Install dependencies

```bash
pip install -r requirements.txt
```

> Dependencies will include: pandas, numpy, matplotlib, seaborn, scikit-learn, statsmodels, geopandas, shapely, folium (optional), etc.

## Status
- [ ] Repo scaffolded
- [ ] Data placed in `data/raw/`
- [ ] Notebook 01: load + reshape Zillow
- [ ] Notebook 02: load + reshape Census
- [ ] Merge + feature engineering
- [ ] EDA + visuals
- [ ] Regression + interpretation
- [ ] Clustering + interpretation
- [ ] Geospatial analysis
- [ ] Tableau dashboard

## Notes for Recruiters
This project emphasizes:
- Clear problem framing
- Reproducible Python workflow
- Data integration (Zillow + Census)
- Honest discussion of limitations
- Stakeholder-friendly storytelling via Tableau
