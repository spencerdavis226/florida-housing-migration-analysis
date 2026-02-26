
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
- Stakeholder-oriented storytelling via Tableau

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
- **Raw File:** `data/raw/CO-EST2024-ALLDATA.csv`
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

---

## Deliverables

- **Tableau Public Dashboard / Storyboard** (link to be added)
- Fully documented and reproducible GitHub repository
- Structured Jupyter notebook workflow

---

## Skills Demonstrated

- Data reshaping (wide-to-long panel transformations)
- Multi-source integration via FIPS join keys
- Time-series feature engineering
- Linear regression modeling (statsmodels)
- Unsupervised learning (k-means clustering)
- Geospatial visualization (GeoPandas)
- Dashboard storytelling (Tableau Public)

---

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
- [ ] Dataset merge + validation
- [ ] Feature engineering
- [ ] EDA + visualization
- [ ] Regression modeling
- [ ] Clustering
- [ ] Geospatial mapping
- [ ] Tableau dashboard

---

## Notes for Recruiters

This project emphasizes:

- Clear problem framing
- Reproducible Python workflows
- Structured multi-dataset integration
- Defensible statistical reasoning
- Transparent discussion of limitations
- Stakeholder-oriented storytelling
