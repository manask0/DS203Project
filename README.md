<div align="center">
 
### DS203 Course Project: Regression
 
*Predicting year-over-year CPI inflation 1, 3, 6 and 12 months ahead from FRED macroeconomic indicators*

 
**Manas Karia · Ishaan Ganesh · Dhruva Vijai**
 
November 2026
 
</div>
---
 
## Overview
 
1. **Exploratory data analysis** to find candidate predictors, redundant features and lead-lag relationships.
2. **Data pipeline** that merged the raw series, engineers stationary features, builds multi-horizon targets and performs a chronological split.
3. **Modelling** where each horizon's model is selected on a validation set and evaluated once on a test set.
## Data
 
All series come from the [Federal Reserve Economic Data (FRED)](https://fred.stlouisfed.org/) repository, covering 1986–2026. 
| Series | Description |
|---|---|
| `CPIAUCSL` | Consumer Price Index (basis of the inflation target) |
| `T10Y2Y` | 10-year minus 2-year Treasury yield spread (daily) |
| `UNRATE` | Unemployment rate |
| `INDPRO` | Industrial production index |
| `FEDFUNDS` | Effective federal funds rate |
| `PAYEMS` | Total nonfarm payrolls |
| `PPIACO` | Producer price index, all commodities |
| `HOUST` | Housing starts |
| `DCOILWTICO` | WTI crude oil price (daily) |
| `M2SL` | M2 money stock |
| `UMCSENT` | University of Michigan consumer sentiment |
 
## Method
 
**Features (14 predictors).** Trending level series (`CPIAUCSL`, `INDPRO`, `PAYEMS`, `PPIACO`, `DCOILWTICO`, `M2SL`, `HOUST`) are converted to year-over-year growth, unemployment is also first differenced, and six series are kept as raw levels (`T10Y2Y`, `UNRATE`, `FEDFUNDS`, `HOUST`, `DCOILWTICO`, `UMCSENT`).
 
**Targets.** The target for horizon *h* is YoY CPI inflation at month *t + h*, for *h* ∈ {1, 3, 6, 12}.
 
**Split.** A chronological 70 / 15 / 15 train-validation-test split with no shuffling and a 12 month gap, so forward looking targets don't cross boundary dates. Scaling is fitted on the training data.
 
| Split | Observations | Period |
|---|---|---|
| Train | 310 | 01-1987 to 10-2012 |
| Validation | 57 | 11-2013 to 07-2018 |
| Test | 70 | 08-2019 to 08-2025 |
 
**Models.** Degree 1 and degree 2 polynomial versions of OLS, Ridge, Lasso and ElasticNet, with regularisation strength α from 10⁻⁴ to 1 and L1 ratios of 0.25, 0.5 and 0.75. For each horizon, the candidate with the lowest validation RMSE is selected.
 
## Results
 
Degree 1 ElasticNet models were selected for all four horizons. Test set performance:
 
| Horizon | α | L1 ratio | MAE | RMSE | R² |
|---|---|---|---|---|---|
| 1 month | 0.03 | 0.75 | 0.437 | 0.576 | 0.941 |
| 3 months | 0.10 | 0.50 | 1.140 | 1.599 | 0.539 |
| 6 months | 0.10 | 0.50 | 1.809 | 2.687 | −0.317 |
| 12 months | 0.10 | 0.75 | 2.184 | 3.117 | −1.052 |


## 📁 Repository Structure
 
```
.
├── Data/              # One CSV per FRED series, named by series ID
├── DS203-Project.ipynb            # Pipeline and modelling notebook
├── report.tex         # Project report (LaTeX)
└── README.md
```
 
##  Team
 
| Name |
|---|
| Dhruva Vijai |
| Manas Karia |
| Ishaan Ganesh |

 
---
 
<div align="center">
*DS203 · Course Project: Regression · November 2026*
 
</div>
 
