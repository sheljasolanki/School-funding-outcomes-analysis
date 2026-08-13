# 📚 School District Funding vs Student Achievement

An end-to-end data analytics project exploring the relationship between **school funding, district poverty, and student academic performance** across U.S. public school districts. The project combines data from multiple government sources, performs extensive data cleaning and feature engineering, applies statistical and machine learning techniques, and presents the findings through an interactive Power BI dashboard.

---

# 📌 Project Overview

Education funding is often considered one of the primary factors influencing student achievement. However, districts with higher spending are frequently those with different socioeconomic characteristics, making it difficult to determine whether spending itself improves outcomes.

This project investigates whether **higher per-pupil expenditure is associated with better academic performance after controlling for district poverty**. Using publicly available U.S. government datasets, a complete analytics pipeline was developed that includes data cleaning, exploratory data analysis, regression modeling, clustering, and dashboard visualization.

The analysis focuses on answering the following question:

> **Does increasing per-pupil spending improve student achievement, or is poverty the stronger predictor of educational outcomes?**

---

# 🎯 Objectives

- Integrate multiple government datasets into a single district-level dataset.
- Clean and validate education, finance, and poverty data.
- Engineer meaningful variables such as per-pupil expenditure and poverty rate.
- Explore relationships between school funding, poverty, and student achievement.
- Build regression models while controlling for poverty.
- Identify different types of school districts using K-Means clustering.
- Develop an interactive Power BI dashboard to communicate insights.

---

# 📊 Datasets Used

| Dataset | Source | Purpose |
|----------|--------|---------|
| EDFacts | U.S. Department of Education | District-level Math and Reading proficiency |
| NCES F-33 Finance Survey | National Center for Education Statistics | School district finance and enrollment |
| SAIPE | U.S. Census Bureau | District-level child poverty estimates |

---

# ⚙️ Project Workflow

```text
Raw Government Datasets
        │
        ▼
Data Cleaning & Validation
        │
        ▼
Feature Engineering
        │
        ▼
Merged District Dataset
        │
 ┌──────┼──────────────┐
 │      │              │
 ▼      ▼              ▼
EDA  Regression   Clustering
 │      │              │
 └──────┼──────────────┘
        ▼
 Power BI Dashboard
```

---

# 🧹 Data Cleaning & Feature Engineering

The raw datasets required extensive preprocessing before they could be analyzed.

The major data preparation steps included:

- Cleaned EDFacts proficiency suppression codes.
- Converted proficiency values into numeric format.
- Converted valid test counts into numeric values.
- Calculated district-level per-pupil expenditure.
- Constructed district poverty rate from SAIPE estimates.
- Removed invalid and incomplete district records.
- Standardized district identifiers (LEAID) across datasets.
- Merged all three government datasets.
- Created separate datasets for regression and clustering.
- Winsorized extreme spending values for regression analysis.

A detailed explanation of every cleaning decision and its rationale is available in the accompanying project report.

---

# 📈 Exploratory Data Analysis

Exploratory analysis was performed to understand the characteristics of the merged dataset before statistical modeling.

The analysis included:

- Summary statistics
- Distribution analysis
- Histogram visualization
- Correlation analysis
- Outlier investigation
- Data quality validation

These analyses helped identify skewed variables, detect unusual observations, and validate that the data were suitable for further modeling.

---

# 📉 Regression Analysis

Ordinary Least Squares (OLS) regression models were built to evaluate the relationship between district funding and student achievement while controlling for poverty.

Two baseline models were developed:

- Math Proficiency Model
- Reading Proficiency Model

Additional analyses included:

- Variance Inflation Factor (VIF) analysis
- Residual diagnostics
- Interaction model (Spending × Poverty)
- Cluster-specific regression
- Prediction and residual analysis

---

# 🤖 Clustering Analysis

K-Means clustering was used to identify natural district archetypes based on educational, financial, and demographic characteristics.

Features used for clustering:

- Per-pupil expenditure
- Poverty rate
- Math proficiency
- Reading proficiency
- Student enrollment

The clustering solution was validated using:

- Elbow Method
- Silhouette Score
- Log-transformation robustness test
- Per-sample silhouette analysis
- Adjusted Rand Index (ARI)

---

# 📊 Key Findings

The analysis produced several important insights:

- District poverty was the strongest predictor of student achievement.
- After controlling for poverty, per-pupil spending showed no statistically significant relationship with Math proficiency.
- Reading proficiency showed a small positive association with spending.
- The relationship between spending and achievement varied across district types.
- Three distinct district archetypes were identified:
  - Low-poverty, high-performing districts
  - High-poverty, lower-performing districts
  - Large urban mega-districts

Together, the regression and clustering analyses suggest that socioeconomic conditions play a larger role in explaining district-level academic outcomes than funding alone.

---

# 📊 Power BI Dashboard

An interactive Power BI dashboard was developed to present the project findings and enable exploration of district-level education data.

The dashboard includes:

- Funding overview
- Academic performance metrics
- Poverty analysis
- District comparison
- Cluster exploration
- Interactive filtering and drill-down

<img width="1376" height="771" alt="image" src="https://github.com/user-attachments/assets/25a85208-9bed-420b-9fc4-97c2c7abf1ab" />
<img width="1319" height="744" alt="image" src="https://github.com/user-attachments/assets/4a5d3001-5c2d-42c9-9cf5-6dbbbd81afbf" />
<img width="1319" height="743" alt="image" src="https://github.com/user-attachments/assets/0f40e62a-8fac-4575-8672-421195d4b411" />
<img width="1318" height="742" alt="image" src="https://github.com/user-attachments/assets/bba1060a-8175-4ab5-a2f5-fcc60b52a8dd" />
<img width="1317" height="745" alt="image" src="https://github.com/user-attachments/assets/c8b338a1-e25d-45f2-a2cd-3d3ee7aa02b1" />


---

# 💻 Technologies Used

### Programming

- Python

### Data Processing

- Pandas
- NumPy

### Data Visualization

- Matplotlib

### Statistical Analysis

- Statsmodels

### Machine Learning

- Scikit-learn

### Business Intelligence

- Power BI

### Development Environment

- Jupyter Notebook

---

# 📌 Future Improvements

Possible extensions of this project include:

- Incorporating additional socioeconomic and demographic variables.
- Expanding the analysis across multiple academic years.
- Comparing linear regression with tree-based machine learning models.
- Performing causal inference using quasi-experimental methods.
- Enhancing the Power BI dashboard with additional policy-focused visualizations.

---

# 📚 References

- U.S. Department of Education – EDFacts
- National Center for Education Statistics (NCES) F-33 Finance Survey
- U.S. Census Bureau – Small Area Income and Poverty Estimates (SAIPE)

