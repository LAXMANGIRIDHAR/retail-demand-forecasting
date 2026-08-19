# Retail Demand Forecasting

An end-to-end machine learning system for forecasting weekly retail sales across stores and departments using historical demand, store characteristics, holidays, markdowns, and economic indicators.

## Project Overview

Retail demand forecasting helps businesses anticipate future sales, improve inventory planning, reduce stockouts, and make better promotional decisions.

This project develops a machine learning pipeline for forecasting weekly sales at the Store + Department + Week level.

The pipeline covers:

- Data ingestion and validation
- Data cleaning and preprocessing
- Exploratory data analysis
- Calendar and holiday feature engineering
- Historical lag features
- Rolling-window statistics
- Time-series-aware validation
- Multiple machine learning models
- Hyperparameter tuning
- Multi-metric evaluation
- Residual and robustness analysis
- SHAP and LIME explainability
- Recursive future forecasting
- Final prediction generation

## Dataset

The project uses the Walmart Store Sales Forecasting dataset.

The dataset contains:

- `stores.csv` — store metadata including store type and size
- `train.csv` — historical weekly sales
- `test.csv` — future observations without the target
- `features.csv` — temperature, fuel price, markdowns, CPI, unemployment, and holiday information

The dataset contains 45 stores covering multiple departments and weekly observations.

## Project Pipeline

```text
Raw CSV Data
      ↓
Data Loading & Validation
      ↓
Data Cleaning & Missing-Value Handling
      ↓
Dataset Merging
      ↓
Exploratory Data Analysis
      ↓
Calendar & Holiday Features
      ↓
Lag & Rolling Features
      ↓
Chronological Train / Validation Split
      ↓
Model Training
      ↓
Linear Regression
Random Forest
Tuned LightGBM
      ↓
Multi-Metric Evaluation
      ↓
Best Model Selection
      ↓
Residual & Robustness Analysis
      ↓
SHAP + LIME Explainability
      ↓
Recursive Test Forecasting
      ↓
Final Weekly Sales Predictions