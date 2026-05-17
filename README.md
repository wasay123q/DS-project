# Predictive Modeling for Retail Inventory Demand

## Overview
This project builds a full data science pipeline to forecast daily retail sales using the Rossmann Store Sales dataset. It emphasizes time‑aware preprocessing, temporal feature engineering, and a fair comparison between a baseline linear model and a tree‑based model using expanding‑window cross‑validation to avoid temporal leakage.

## Course
CS402 Data Science Semester Project

## Team
- Abdul wasay Sial
- Faseeh Anjum
- Hasan Raza

## Dataset
**Rossmann Store Sales** (Kaggle)
- Required files:
  - data/train.csv
  - data/store.csv

> The dataset is not committed to this repository. Download it from Kaggle and place the CSV files in the data/ folder.

## Key Steps (Pipeline)
1. **Ingestion & Merge**: Join train.csv with store.csv by Store ID and sort chronologically.
2. **Preprocessing**:
   - Missing value handling for competition and promo fields.
   - Temporal features: Year, Month, DayOfWeek, IsWeekend.
   - Categorical encoding for StoreType and Assortment.
   - Lag features: 7‑day and 30‑day sales.
3. **Advanced Prep**:
   - Outlier capping via IQR.
   - Standardization of numeric lag and competition distance features.
4. **EDA**: 8 plots covering distribution, seasonality, promo impact, correlations, and competitor distance.
5. **Modeling**:
   - Baseline: Linear Regression
   - Advanced: Random Forest Regressor
   - Validation: Expanding‑window `TimeSeriesSplit`
6. **Interpretation & Export**:
   - Feature importance visualization
   - Model saved as joblib

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

## Notes
- The notebook samples 250,000 rows for faster demo training.
- If you want full‑data training, remove or adjust the sampling line in `train_and_evaluate`.

## License
For academic use only unless otherwise specified.
