![Python](https://img.shields.io/badge/Python-3.11-blue)
![Prophet](https://img.shields.io/badge/Prophet-Time%20Series-orange)
![SARIMAX](https://img.shields.io/badge/SARIMAX-Statsmodels-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

# Business KPI Forecasting Pipeline (Prophet + SARIMAX Ensemble)

Production-style time series forecasting pipeline for business KPIs using an ensemble of Prophet and SARIMAX with automated data preprocessing, anomaly detection, hyperparameter tuning, model evaluation, and 12-month forecasting.

---

## Project Highlights

- Automated forecasting for multiple business KPIs
- Ensemble of Prophet and SARIMAX
- Robust preprocessing with anomaly detection
- Hyperparameter tuning using time-series cross-validation
- Dynamic 12-month forecasting horizon
- Production-ready modular architecture

---

## Project Overview

This project implements an end-to-end forecasting pipeline for monthly business metrics.

The pipeline was designed to automate forecasting for business partners with different historical data lengths while maintaining high forecast quality and minimizing manual parameter tuning.

The forecasting process combines two complementary models:

- **Prophet** — captures trend and external seasonality
- **SARIMAX** — models autoregressive and seasonal dependencies

The final prediction is produced as an **ensemble**, where the optimal weights are selected automatically based on validation performance.

---

## Business Problem

Many business metrics exhibit:

- long-term trend
- annual seasonality
- irregular spikes
- missing observations
- relatively short historical datasets

Using only one forecasting model often leads to unstable results.

This project addresses these challenges by combining statistical and machine learning approaches into a robust forecasting pipeline.

---

## Forecasted Metrics

The pipeline forecasts:

- Number of Clients
- Number of Transactions
- Average Check

Based on these predictions it additionally calculates:

- Revenue
- Purchase Frequency

---

## Pipeline Architecture

```
Load Data
      │
      ▼
Data Cleaning
      │
      ▼
Missing Value Imputation
      │
      ▼
Outlier Detection
      │
      ▼
Feature Preparation
      │
      ▼
Train/Test Split
      │
      ▼
Hyperparameter Tuning
      │
      ▼
Train Prophet
      │
      ▼
Train SARIMAX
      │
      ▼
Model Evaluation
      │
      ▼
Ensemble Selection
      │
      ▼
12-Month Forecast
      │
      ▼
Business KPI Calculation
      │
      ▼
Excel Export
```

---

# Data Preprocessing

Before training, the pipeline automatically performs:

- Monthly time index reconstruction
- Missing month insertion
- Missing value imputation
- Outlier detection using rolling Median Absolute Deviation (MAD)
- Linear time interpolation
- Forward/backward filling
- Data type validation
- Negative value handling

A detailed cleaning report is generated for every corrected observation.

---

# External Seasonality

Instead of relying on Prophet's built-in yearly seasonality, the pipeline uses an external seasonality table containing monthly coefficients.

This allows the model to incorporate business-specific seasonal effects while avoiding duplicated seasonality estimation.

---

# Prophet Model

Prophet is configured with:

- custom changepoint flexibility
- custom seasonality strength
- external seasonal regressors
- automatic hyperparameter tuning using rolling time-series validation

Parameters tuned:

- changepoint_prior_scale
- seasonality_prior_scale

---

# SARIMAX Model

The statistical forecasting component includes:

- automatic ARIMA initialization (AutoARIMA when available)
- localized grid search
- seasonal ARIMA modeling
- AIC-based model selection

For short time series the search space is reduced automatically to improve stability.

---

# Ensemble Strategy

The final forecast is calculated as

Forecast = w × Prophet + (1 − w) × SARIMAX

The ensemble weight is selected automatically using validation MAE.

This approach allows the pipeline to adapt to different metric behaviors without manual intervention.

---

# Validation Strategy

The project uses:

- chronological train/test split
- rolling time-series cross-validation
- adaptive folds for short historical series

Evaluation metrics:

- MAE
- RMSE
- MAPE

---

# Forecast Horizon

The forecast horizon is generated dynamically.

Instead of hardcoding calendar dates, the pipeline automatically forecasts **12 months after the last available observation**.

Example:

Historical data:

```
May 2024
...
May 2026
```

Forecast:

```
June 2026
...
May 2027
```

---

# Visualizations

The notebook generates two diagnostic charts for every metric.

### Model validation

Displays:

- training period
- test period
- Prophet prediction
- SARIMAX prediction
- Ensemble prediction

allowing visual comparison of model performance.

---

### Future Forecast

Displays:

- historical observations
- 12-month forecast
- confidence interval
- forecast boundary

---

# Output

The pipeline exports:

- forecast for every KPI
- lower confidence bound
- upper confidence bound
- Prophet prediction
- SARIMAX prediction

The final result is automatically saved to Excel.

---

# Technologies

- Python
- Pandas
- NumPy
- Prophet
- Statsmodels
- SARIMAX
- AutoARIMA
- Scikit-learn
- Matplotlib

---

# Repository Structure

```
.
│
├── Forecast.ipynb
├── data/
│   ├── partner_data.xlsx
│   └── seasonality.xlsx
│
├── output/
│   └── forecast.xlsx
│
└── README.md
```

---

# Key Features

- Production-style forecasting pipeline
- Automated preprocessing
- Robust anomaly detection
- Missing value imputation
- External business seasonality
- Adaptive parameter tuning
- Prophet + SARIMAX ensemble
- Automatic model evaluation
- Dynamic forecast horizon
- Automatic Excel export
- Modular and reusable codebase

---

# Future Improvements

Potential future enhancements include:

- Optuna-based Bayesian hyperparameter optimization
- Prediction interval calibration
- MLflow experiment tracking
- Automatic model selection
- Holiday effects
- Additional external regressors
- Confidence interval estimation for the ensemble
- Interactive dashboard (Streamlit)

---

# Author

Irina — Data Analyst

This project was created as a portfolio demonstration of building an end-to-end business forecasting pipeline using statistical and machine learning techniques.
