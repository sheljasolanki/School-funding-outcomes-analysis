# School Funding & Student Outcomes Analysis

An analysis of whether per-pupil school spending predicts student academic proficiency once district poverty is controlled for — using three federal datasets covering nearly every public school district in the United States.

## Project Overview

School funding is a widely debated policy topic, but the relationship between spending and outcomes is rarely tested directly against real data. This project combines district-level financial records, poverty estimates, and academic proficiency data to answer two questions:

1. Does per-pupil spending predict proficiency after controlling for poverty?
2. Are there natural groupings (archetypes) of districts that share similar financial and academic profiles?

## Data Sources

| Source | Provider | Data |
|---|---|---|
| **EDFacts** | U.S. Department of Education | District-level math and reading proficiency rates |
| **F-33** (School District Finance Survey) | U.S. Census Bureau (for NCES) | District-level revenue and expenditure data |
| **SAIPE** (Small Area Income and Poverty Estimates) | U.S. Census Bureau | District-level child poverty and income estimates |

These three sources had no shared file structure, and SAIPE had no ready-made district ID — a significant part of this project involved cleaning, standardizing, and merging them into one trustworthy analysis table.

## Repository Structure
├── datasets/
│ ├── district_data_full.csv # Full merged dataset (unfiltered, used for clustering/PBI)
│ ├── district_data_regression.csv # Regression-filtered dataset (enrollment/population thresholds + capped spending)
│ ├── district_data_with_clusters.csv # Final dataset with k=3 cluster labels
│ ├── district_residuals.csv # Regression residuals for diagnostics
│ ├── finance.xlsx # Raw F-33 finance data
│ ├── math.csv # Raw EDFacts math proficiency data , Not added due to size limit on github
│ ├── Reading.csv # Raw EDFacts reading proficiency data , Not added due to size limit on github
│ └── saipe.xlsx # Raw SAIPE poverty data
├── notebooks/
│ ├── data.ipynb # Data cleaning and merging
│ ├── Regression.ipynb # Baseline, interaction, and per-cluster regression models
│ └── clustering.ipynb # K-means clustering and robustness checks
├── PBI/
│ └── SCHOOL PROJECT.pbix # Power BI dashboard
└── README.md

## Methodology

### Data Cleaning
- Converted EDFacts privacy-suppression codes (ranges, bounded codes, full suppression) into estimated numeric values rather than dropping ~32% of rows, avoiding bias toward larger districts.
- Removed non-school agencies and unreported-finance rows from F-33 using NCES's documented placeholder codes.
- Applied a minimum enrollment threshold to remove specialized institutions and unstable per-pupil outliers from finance data.
- Reconstructed a matching district ID (LEAID) for SAIPE by concatenating State FIPS and District ID.
- Merged all three sources on LEAID, resulting in a final dataset of **12,158 districts** with zero missing values.
- Maintained two parallel datasets: a full, unfiltered version for clustering/visualization, and a filtered, capped version for regression, since small/high-spending districts are a meaningful pattern for clustering but a stability risk for regression coefficients.

### Regression Analysis
- Baseline OLS models (math and reading) testing proficiency against per-pupil spending and poverty rate.
- Diagnostics: VIF (multicollinearity), residual plots (homoscedasticity), Jarque-Bera test (normality).
- Extended with a centered interaction term (spending × poverty) and per-cluster regression to test whether spending's effect varies by poverty level and district archetype.

### Clustering Analysis
- K-means clustering on five standardized features (spending, poverty, math/reading proficiency, enrollment).
- Cluster count validated via silhouette score (k=3 over an ambiguous elbow-method estimate of k=4).
- Robustness checks: log-transform test, per-sample silhouette analysis, and stability across random seeds (Adjusted Rand Index).

## Key Findings

- **Poverty is the dominant, consistent predictor** of academic proficiency across all models (p < 0.001).
- **Per-pupil spending's effect is smaller, inconsistent across subjects**, and conditional on poverty level — positive only in lower-poverty districts, weakening or reversing as poverty increases.
- Clustering identified **three district archetypes**: affluent high-performers, high-poverty under-performers, and large-scale, lower-cost mega-districts.
- Spending is not a uniform lever for improving outcomes — its relationship with proficiency depends heavily on district socioeconomic context.

## Tools Used

- **Python** (pandas, statsmodels, scikit-learn) — data cleaning, regression, clustering
- **SQL** — summary queries on the cleaned dataset
- **Power BI** — interactive dashboard for exploring results

## Limitations

- Suppressed proficiency values are estimated via midpoint conversion, not true reported values.
- Regression stability thresholds (enrollment ≥ 100, school-age population ≥ 100) exclude some legitimate small districts.
- R² values are modest, consistent with academic outcomes being influenced by many unmeasured factors beyond spending and poverty.
- The mega-district cluster (n=34) is small; its per-cluster regression estimates should be treated as suggestive, not conclusive.
