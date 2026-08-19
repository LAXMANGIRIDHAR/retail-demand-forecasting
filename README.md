# Walmart Store Sales --- Demand Forecasting

**AI Engineer Technical Assignment --- NEUZENAI IT SOLUTIONS PVT LTD**

## Project Overview

This project forecasts weekly Walmart sales at Store + Department + Week
granularity using historical sales, store metadata, economic indicators,
markdowns, holidays, and engineered time-series features.

The final v2 notebook implements data ingestion, cleaning, EDA,
calendar/holiday/lag/rolling features, chronological validation, Linear
Regression, Random Forest, tuned LightGBM, aggregate Prophet/LSTM
experiments, multi-metric evaluation, residual analysis, robustness
testing, SHAP/LIME explainability, recursive test forecasting, and
Kaggle-format submission generation.

## Final Result

**Selected model: Random Forest --- selected programmatically by minimum
WMAE.**

  ------------------------------------------------------------------------------------
  Model                   MAE           RMSE           WMAE         MAPE            R²
  ------------ -------------- -------------- -------------- ------------ -------------
  **Random       **1,371.55**       2,875.41   **1,400.53**       59.40%       0.98553
  Forest**                                                               

  LightGBM           1,407.72   **2,866.75**       1,456.44      170.93%   **0.98562**
  (tuned)                                                                

  Linear             1,731.64       3,323.84       1,757.65      423.51%       0.98067
  Regression                                                             
  ------------------------------------------------------------------------------------

WMAE is the primary metric because holiday observations receive 5×
weight.

## Dataset

Files: - `stores.csv` - `train.csv` - `test.csv` - `features.csv`

The executed notebook records 45 stores, 207,323 training rows, 115,064
test rows, and 8,190 feature rows.

## Repository Structure

``` text
walmart-demand-forecasting/
├── data/
│   ├── stores.csv
│   ├── train.csv
│   ├── test.csv
│   └── features.csv
├── notebooks/
│   └── demand_forecasting_v2(1).ipynb
├── reports/
│   ├── process_flow_document.pdf
│   ├── model_comparison_report.pdf
│   └── submission.csv
├── README.md
└── requirements.txt
```

## Pipeline

``` text
Raw CSVs
  ↓
Data Loading & Validation
  ↓
Store + Feature Merge
  ↓
Cleaning / Imputation
  ↓
EDA
  ↓
Calendar + Holiday + Lag + Rolling Features
  ↓
Chronological Train / Validation Split
  ↓
Linear Regression | Random Forest | Tuned LightGBM
  ↓
MAE / RMSE / WMAE / MAPE / R²
  ↓
Best Model = Random Forest
  ↓
Residual + Robustness Analysis
  ↓
SHAP + LIME Explainability
  ↓
Recursive Test Forecasting
  ↓
reports/submission.csv
```

## Feature Engineering

Calendar/holiday: - `Year` - `Week` - `Month` - `Days_to_Holiday` -
`Near_Holiday`

Historical demand per Store + Department: - `Sales_lag_1` -
`Sales_lag_2` - `Sales_lag_4` - `Sales_lag_52` - `Sales_roll_mean_4` -
`Sales_roll_mean_8` - `Sales_roll_mean_12`

External/store variables include Store, Department, Size, Temperature,
Fuel_Price, CPI, Unemployment, MarkDown1--5 and Store Type.

## Validation

Primary chronological split: - Training through **2012-07-20** -
Validation from **2012-07-27**

LightGBM tuning uses 5-fold expanding-window `TimeSeriesSplit`.

## Robustness

Validation WMAE: - Non-holiday: **1,361.52** - Holiday: **1,502.14** -
Store Type A: **1,739.93** - Store Type B: **1,073.77** - Low markdown:
**708.88** - Medium markdown: **829.05** - High markdown: **1,471.81**

## Explainability

Top SHAP drivers: 1. `Sales_lag_52` 2. `Sales_lag_1` 3.
`Sales_roll_mean_4` 4. `Sales_roll_mean_8` 5. `Sales_roll_mean_12` 6.
`Sales_lag_2` 7. `Sales_lag_4` 8. `Unemployment` 9. `Days_to_Holiday`
10. `MarkDown3`

The notebook generates global SHAP plots, local SHAP explanations for
three predictions, and a LIME local cross-check.

## Test Forecasting

Because `test.csv` has no target sales, the notebook performs recursive
walk-forward forecasting:

1.  Start with training history.
2.  Build lag/rolling features for the first test week.
3.  Predict the week.
4.  Append the prediction to history.
5.  Build the next week's features.
6.  Continue through the test horizon.

The notebook covers 39 test weeks and produces 115,064 forecasts.

Output: `reports/submission.csv`

## Reproduction

Recommended Python: **3.11**

``` bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Open:

`notebooks/demand_forecasting_v2(1).ipynb`

Run all cells top-to-bottom.

### Path note

The current notebook defines `DATA_DIR = '../data'`, but the CSV-loading
lines use filenames directly. For exact reproduction, either run the
notebook with the CSVs in its working directory or update the four read
paths to use `DATA_DIR`.

## Limitations

-   Holiday and Type A errors are higher than their comparison groups.
-   Recursive forecasting can propagate prediction errors into later lag
    features.
-   Current cold-start fallback fills unavailable lag/rolling features
    with zero; production should use hierarchical/store-type/department
    fallback estimates.
-   Residual distribution is visually inspected; no formal normality
    test is implemented.
-   Future improvements: holiday-specific models, hierarchical
    forecasting, richer external data, ensembling and automated drift
    monitoring.

## Deliverables

-   `demand_forecasting_v2(1).ipynb`
-   `process_flow_document.pdf`
-   `model_comparison_report.pdf`
-   `README.md`
-   `requirements.txt`
