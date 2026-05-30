# Predictive Modeling for Retail Inventory Demand

## Overview
This project delivers an end‑to‑end data science pipeline to forecast daily retail sales (inventory demand proxy) using the Rossmann Store Sales dataset. It emphasizes time‑aware preprocessing, robust temporal feature engineering, and a fair comparison between a linear baseline and a tree‑based model using expanding‑window cross‑validation to prevent temporal leakage.

## Course
CS402 Data Science Semester Project

## Team
- Abdul wasay Sial
- Faseeh Anjum
- Hasan Raza

## Problem Statement
Accurate demand forecasting is critical for inventory planning and promotional strategy. This project predicts daily store sales using historical transactions and store metadata while respecting time‑series constraints.

## Objectives
- Build a reproducible pipeline from raw data to trained model.
- Engineer time‑series features that capture seasonality and lag effects.
- Compare a simple baseline model with a stronger non‑linear model.
- Use time‑aware validation to avoid leakage and over‑optimistic metrics.
- Produce interpretable outputs (EDA plots and feature importance).

## Dataset
**Rossmann Store Sales** (Kaggle)
- Required files:
  - data/train.csv
  - data/store.csv

> The dataset is not committed to this repository. Download it from Kaggle and place the CSV files in the data/ folder.

### Expected Columns (High‑Level)
- **train.csv**: Store, Date, Sales, Customers, Open, Promo, StateHoliday, SchoolHoliday
- **store.csv**: Store, StoreType, Assortment, CompetitionDistance, CompetitionOpenSince*, Promo2, Promo2Since*, PromoInterval

## Methodology (Pipeline)
1. **Ingestion & Merge**
   - Load transactional data and store metadata.
   - Left join on Store ID and sort chronologically.
2. **Core Preprocessing**
   - Missing value handling for competition and promo fields.
   - Temporal features: Year, Month, DayOfWeek, IsWeekend.
   - Categorical encoding for StoreType and Assortment.
   - Lag features: 7‑day and 30‑day sales by store.
3. **Advanced Preprocessing**
   - Outlier capping via IQR (Sales).
   - Standardization of numeric features for stability.
4. **Exploratory Data Analysis (EDA)**
   - 8 visualizations covering distribution, seasonality, promo impact, correlations, store types, and competition distance.
5. **Modeling & Validation**
   - Baseline: Linear Regression.
   - Advanced: Random Forest Regressor.
   - Validation: Expanding‑window `TimeSeriesSplit`.
   - Metrics: RMSE and MAE.
6. **Interpretation & Export**
   - Feature importance visualization.
   - Exported model in joblib format.

## Feature Engineering Details
- **Temporal decomposition**: Year, Month, DayOfWeek, IsWeekend.
- **Lag features**: Sales_Lag_7 and Sales_Lag_30 per store to encode recent demand signals.
- **Categorical encoding**: StoreType and Assortment mapped to numeric levels.
- **Outlier handling**: IQR‑based capping on Sales to reduce extreme influence.
- **Scaling**: Standardization of numeric lag and competition distance features.

## Validation Strategy
Time‑series data requires respecting temporal order. The notebook uses expanding‑window cross‑validation (`TimeSeriesSplit`) so that each fold trains on past data and tests on future data. This avoids leakage that would occur with random splits.

## Evaluation Metrics
- **RMSE**: Penalizes larger errors, sensitive to outliers.
- **MAE**: Interpretable average absolute error in sales units.

> Add your final metric values in the notebook output or a Results section if required for submission.

## Project Structure
```
.
├─ data/
│  ├─ store.csv
│  └─ train.csv
├─ notebooks/
│  ├─ notebook.ipynb
│  ├─ models/
│  │  └─ retail_rf_model.joblib
│  └─ visualizations/
├─ requirements.txt
└─ README.md
```

## Setup
1. Create and activate a Python environment.
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

## Reproducibility
- Ensure data files are placed exactly in data/.
- Run the notebook from top to bottom.
- Outputs are saved into notebooks/visualizations/ and notebooks/models/.

## Usage
1. Open notebooks/notebook.ipynb
2. Run cells in order to:
   - preprocess data
   - generate EDA plots
   - train models
   - export the final Random Forest model

## Outputs
- **Visualizations**: notebooks/visualizations/
- **Trained model**: notebooks/models/retail_rf_model.joblib

## Results (Update After Running)
Add a short summary of final RMSE/MAE for both models here once you run the notebook:
- Linear Regression: RMSE = ___, MAE = ___
- Random Forest: RMSE = ___, MAE = ___

## Reproducibility Notes
- The notebook samples 250,000 rows for faster demo training.
- For full‑data training, remove or adjust the sampling line in `train_and_evaluate`.

## Repository Policy
- Raw datasets are not tracked in Git.
- Generated outputs (plots and trained model) are tracked under notebooks/ for presentation/demo purposes.

## References
- Kaggle: Rossmann Store Sales
- scikit‑learn documentation (TimeSeriesSplit, RandomForestRegressor)

## License
For academic use only unless otherwise specified.
