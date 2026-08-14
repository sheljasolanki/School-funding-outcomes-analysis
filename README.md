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

```
├── datasets/
│   ├── district_data_full.csv           # Full merged dataset (unfiltered, used for clustering/PBI)
│   ├── district_data_regression.csv     # Regression-filtered dataset (enrollment/population thresholds + capped spending)
│   ├── district_data_with_clusters.csv  # Final dataset with k=3 cluster labels
│   ├── district_residuals.csv           # Regression residuals for diagnostics
│   ├── finance.xlsx                     # Raw F-33 finance data
│   ├── math.csv                         # Raw EDFacts math proficiency data, not added due to GitHub size limit
│   ├── Reading.csv                      # Raw EDFacts reading proficiency data, not added due to GitHub size limit
│   └── saipe.xlsx                       # Raw SAIPE poverty data
├── notebooks/
│   ├── data.ipynb                       # Data cleaning and merging
│   ├── Regression.ipynb                 # Baseline, interaction, and per-cluster regression models
│   └── clustering.ipynb                 # K-means clustering and robustness checks
├── PBI/
│   └── SCHOOL PROJECT.pbix              # Power BI dashboard
└── README.md
```

## Methodology

### Data Cleaning

**EDFacts proficiency suppression handling**
- Converted privacy-suppression codes (ranges, bounded codes like `LE10`/`GE95`, full suppression `PS`) into estimated midpoint values instead of dropping the ~32% of affected rows, avoiding bias toward larger districts.
- Verified the resulting missing-value count exactly matched the original `PS` count — no data silently lost or misclassified.

**Finance (F-33) cleaning**
- Removed non-school agencies and unreported-finance rows using NCES's documented placeholder codes (`-1` = not reported, `-2` = not applicable).
- Applied a minimum enrollment threshold (50 students) to separate specialized institutions and unstable per-pupil outliers from genuine small districts.

**SAIPE poverty rate construction**
- Reconstructed a matching district ID (LEAID) by concatenating State FIPS Code and District ID, verified against a known district present in all three sources.
- Excluded districts with fewer than 100 school-age children, since very small populations produced statistically unstable poverty rate estimates.

**Final merge**
- Merged EDFacts, finance, and SAIPE sequentially on `LEAID`, resulting in a final dataset of **12,158 districts** with zero missing values.
- Maintained two parallel datasets: a full, unfiltered version for clustering/visualization (since small, high-spending districts are a meaningful pattern worth discovering), and a filtered, capped version for regression (enrollment ≥ 100, school-age population ≥ 100, spending capped at the 99th percentile) to prevent unstable small-denominator values from distorting coefficients.

### Regression Analysis
- Baseline OLS models (math and reading) testing proficiency against per-pupil spending and poverty rate.
- Diagnostics: VIF (multicollinearity), residual plots (homoscedasticity), Jarque-Bera test (normality).
- Extended with a centered interaction term (spending × poverty) and per-cluster regression to test whether spending's effect varies by poverty level and district archetype.

### Clustering Analysis
- K-means clustering on five standardized features (spending, poverty, math/reading proficiency, enrollment), run on the full unfiltered dataset.
- Cluster count validated via silhouette score (k=3 over an ambiguous elbow-method estimate of k=4).
- Robustness checks: log-transform test, per-sample silhouette analysis, and stability across random seeds (Adjusted Rand Index).

## Key Findings

**Regression:**
- Poverty rate: highly significant in both models (p < 0.001) — each 1-point increase in poverty associated with a **0.94-point decrease in math proficiency** and a **0.99-point decrease in reading proficiency**.
- Per-pupil spending: **not significant for math** (p = 0.255); small but significant positive effect for reading (p = 0.001, minimal practical magnitude).
- Diagnostics: VIF ≈ 1.05 for both predictors (no multicollinearity); residuals reasonably homoscedastic; Jarque-Bera test showed mild non-normality (Prob(JB) = 5.26e-28), not disqualifying given n≈12,000.
- Interaction term (spending × poverty, centered to fix a high condition number — 1.72e+06 raw → 6.29e+04 centered): significant and negative in both models (p < 0.001) — spending shows a positive relationship with proficiency only below ~13% poverty rate; above that threshold, the relationship turns negative and continues to weaken further as poverty rises.
- Per-cluster regression: spending's negative association grows from Cluster 0 (-0.0001) → Cluster 1 (-0.00038) → Cluster 2 (-0.0014), broadly consistent with the interaction finding. It's worth noting this isn't perfectly explained by poverty alone — Cluster 2's poverty rate (17.77%) is actually lower than Cluster 1's (21.32%), yet Cluster 2 shows the most negative coefficient. This suggests Cluster 2's result also reflects something specific to being a large-scale urban district, not poverty alone; combined with its small sample size (n=34), it should be treated as suggestive rather than precise.

**Clustering (k=3, validated via silhouette score 0.321 vs. 0.236 for k=4):**

| Cluster | Description | Poverty Rate | Per-Pupil Spending | Math Prof. | Reading Prof. | Enrollment | n |
|---|---|---|---|---|---|---|---|
| 0 | Low-poverty, well-resourced, high-performing | 10.51% | $18,942.43 | 58.73% | 63.39% | 3,459.85 | 5,945 |
| 1 | High-poverty, under-performing | 21.32% | $15,809.47 | 32.52% | 38.38% | 2,954.79 | 6,789 |
| 2 | Large urban mega-districts | 17.77% | $14,180.71 | 46.68% | 48.79% | 168,610.68 | 34 |

- Robustness: log-transformed enrollment tested but reduced separation (silhouette at k=3: 0.321 → 0.268), so raw enrollment was retained; per-sample silhouette showed no negative values in the mega-district cluster despite its small size; stability confirmed across random seeds (Adjusted Rand Index: 0.994–1.000).
- Poverty is the dominant, consistent predictor of proficiency across all models; spending's effect is smaller, subject-dependent, and conditional on poverty level.

## Power BI Dashboard
<img width="1376" height="771" alt="Screenshot 2026-08-13 190050" src="https://github.com/user-attachments/assets/065b6bb6-7d65-4716-bb6b-77eb15fffc92" />
<img width="1319" height="744" alt="Screenshot 2026-08-13 190135" src="https://github.com/user-attachments/assets/c34df08b-a50a-4dd9-b4ba-ef3291e2e753" />
<img width="1319" height="743" alt="Screenshot 2026-08-13 190153" src="https://github.com/user-attachments/assets/091570e7-4ef0-4ae8-a10f-226f68676629" />
<img width="1318" height="742" alt="Screenshot 2026-08-13 190213" src="https://github.com/user-attachments/assets/e12bc5be-bc11-4aa6-a58a-af8a4110de76" />
<img width="1317" height="745" alt="Screenshot 2026-08-13 190230" src="https://github.com/user-attachments/assets/99e23c2a-a4cc-41c0-8adc-6b2721845859" />

## Tools Used

- **Python** — data cleaning and merging (pandas)
- **OLS Regression** (statsmodels) — baseline, interaction, and per-cluster models
- **Machine Learning / K-Means Clustering** (scikit-learn) — district archetype identification
- **Power BI** — interactive dashboard for exploring results

## Limitations

- Suppressed proficiency values are estimated via midpoint conversion, not true reported values.
- Regression stability thresholds (enrollment ≥ 100, school-age population ≥ 100) exclude some legitimate small districts.
- R² values are modest, consistent with academic outcomes being influenced by many unmeasured factors (teacher quality, family circumstances, etc.) beyond spending and poverty.
- The mega-district cluster (n=34) is already small before any regression filtering; its per-cluster regression ran on an even smaller subset, so those estimates should be treated as suggestive, not conclusive.
- This is an observational, cross-sectional analysis — it identifies statistical associations, not causal relationships. Reverse causality and omitted variables (e.g., teacher quality, local governance) cannot be ruled out.

## Conclusion

Poverty is the dominant, consistent predictor of academic proficiency across all models. Per-pupil spending's effect is smaller, inconsistent across subjects, and conditional on poverty level — showing a modest positive relationship only in lower-poverty districts, and weakening or reversing as poverty increases. Clustering independently identified three coherent district archetypes — affluent high-performers, high-poverty under-performers, and large-scale, lower-cost mega-districts — and the interaction/per-cluster regression results reinforced the same core finding from a different analytical angle: spending is not a uniform lever for improving outcomes, and its relationship with proficiency depends heavily on the socioeconomic context of the district.
