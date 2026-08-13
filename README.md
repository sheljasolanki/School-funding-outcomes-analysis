📚 School District Funding vs Student Achievement
📌 Project Overview

This project investigates whether higher school funding leads to better student academic performance after accounting for district poverty. Using publicly available U.S. government datasets, I built a complete data analysis pipeline involving data cleaning, exploratory data analysis, regression modeling, clustering, and interactive visualization.

The project answers the question:

Does increasing per-pupil spending improve student achievement, or is poverty a stronger predictor of educational outcomes?

🎯 Objectives
Merge multiple education datasets into one analysis-ready dataset.
Clean and validate government education data.
Analyze the relationship between school funding and student achievement.
Control for poverty using regression analysis.
Identify different types of school districts using clustering.
Create an interactive Power BI dashboard for exploration.
📊 Datasets Used
Dataset	Purpose
EDFacts	District-level Math & Reading proficiency
NCES F-33 Finance	School district revenue and expenditure
SAIPE	District poverty estimates
🛠️ Project Workflow
Raw Government Data
        │
        ▼
Data Cleaning & Validation
        │
        ▼
Merged Analysis Dataset
        │
        ├─────────────► Exploratory Data Analysis
        │
        ├─────────────► Regression Analysis
        │
        ├─────────────► K-Means Clustering
        │
        ▼
Power BI Dashboard
🧹 Data Cleaning

Major preprocessing steps included:

Cleaned EDFacts suppression codes (ranges, LE/LT, GE, PS)
Converted proficiency values into usable numeric format
Constructed poverty rate from SAIPE estimates
Calculated per-pupil expenditure
Removed invalid districts
Merged all three datasets using LEAID
Created separate datasets for regression and visualization
Winsorized extreme spending values for regression
📈 Exploratory Data Analysis

Performed:

Summary statistics
Distribution analysis
Correlation analysis
Outlier investigation
Data quality validation
📉 Regression Analysis

Built Ordinary Least Squares (OLS) regression models to evaluate the relationship between:

Per-pupil spending
Poverty rate

and student proficiency.

Additional analyses included:

Multicollinearity check (VIF)
Residual diagnostics
Interaction model (Spending × Poverty)
Cluster-specific regression
Prediction and residual analysis
🤖 Clustering Analysis

Applied K-Means clustering to identify district archetypes.

Validation methods:

Elbow Method
Silhouette Score
Log-transformation robustness test
Per-sample silhouette analysis
Adjusted Rand Index (ARI) stability testing
📊 Key Findings
Poverty was the strongest predictor of student achievement.
Per-pupil spending had no significant relationship with Math proficiency after controlling for poverty.
Reading proficiency showed a small positive relationship with spending.
Spending effects varied depending on district poverty levels.
Three distinct district archetypes were identified:
Low-poverty, high-performing districts
High-poverty, lower-performing districts
Large urban mega-districts
💻 Technologies Used
Python
Pandas
NumPy
Matplotlib
Scikit-learn
Statsmodels
Power BI
Jupyter Notebook
📂 Repository Structure
School-Funding-Analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── data_cleaning.ipynb
│   ├── regression.ipynb
│   └── clustering.ipynb
│
├── dashboard/
│   └── School_Dashboard.pbix
│
├── images/
│   ├── dashboard.png
│   ├── elbow.png
│   └── correlation.png
│
├── README.md
└── requirements.txt
📷 Dashboard Preview

(Insert screenshots of your Power BI dashboard here.)

🚀 How to Run
git clone https://github.com/yourusername/school-funding-analysis.git

cd school-funding-analysis

pip install -r requirements.txt

Run the notebooks in order:

1. data_cleaning.ipynb
2. regression.ipynb
3. clustering.ipynb
📌 Future Improvements
Incorporate teacher quality and demographic variables.
Perform longitudinal analysis using multiple years of data.
Compare linear regression with tree-based models.
Expand the dashboard with additional policy metrics.
