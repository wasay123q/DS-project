# Notebook Cell-by-Cell Explanation

This document explains each cell in notebooks/notebook.ipynb in the order it appears. It focuses on what the code does, the key variables it creates, and the outputs it produces.

## Cell 1 (Markdown) - Project title and objective
- Introduces the course project, the full project title, and the team members.
- States the objective: build a complete pipeline to forecast daily sales on the Rossmann dataset, using time-aware validation to prevent leakage.

## Cell 2 (Code) - Environment setup and imports
- Imports core libraries for data handling (pandas, numpy), visualization (matplotlib, seaborn), and modeling (scikit-learn).
- Imports utilities like `joblib` for model saving, `warnings` for cleaner output, and `os` for file system tasks.
- Suppresses warnings to reduce noise in outputs.
- Sets a consistent plotting theme and default figure size.
- Prints a confirmation message so you can verify the cell ran successfully.

## Cell 3 (Markdown) - Data ingestion overview
- Explains that `train.csv` (transactions) and `store.csv` (metadata) are merged using the `Store` key.
- Emphasizes that a left join mirrors standard relational data preparation.

## Cell 4 (Code) - Data loading and merging
- Defines `load_and_merge_data()` to:
  - Read `train.csv` with date parsing for the `Date` column.
  - Read `store.csv` normally.
  - Merge on `Store` using a left join, keeping all transactions.
  - Sort by `Date` and `Store` to preserve chronological order.
- Executes the function, storing the merged data in `raw_df`.
- Prints the merged dataset shape for validation.

## Cell 5 (Markdown) - Preprocessing plan
- Describes three preprocessing goals:
  - Missing value handling (for competition and promo features).
  - Temporal feature extraction (Year, Month, DayOfWeek).
  - Categorical transformation.
- Note: The text mentions "target encoding", but the code implements ordinal mapping (simple numeric mapping), not target encoding.

## Cell 6 (Code) - Preprocessing and advanced preprocessing
This cell contains two major parts: a core preprocessing pipeline and an advanced preprocessing step.

### Part A: `preprocess_pipeline(df)`
- Makes a copy of the input to avoid in-place changes.
- Fills missing `CompetitionDistance` with a large value (max * 2) to represent far competitors.
- Fills promo and competition start fields with 0 when missing.
- Replaces missing `PromoInterval` with the string `None`.
- Creates time features from `Date`: `Year`, `Month`, `DayOfWeek`, and `IsWeekend`.
- Filters out rows where the store was closed or sales are non-positive.
- Encodes `StoreType` and `Assortment` to numeric levels.
- Creates lag features per store:
  - `Sales_Lag_7` (sales shifted by 7 days)
  - `Sales_Lag_30` (sales shifted by 30 days)
- Fills missing lag values with 0 after shifting.
- Replaces infinite values with NaN, then fills numeric NaNs with the median.
- Drops non-numeric or target-leakage columns (`Date`, `Customers`, `StateHoliday`, `PromoInterval`, `Open`).
- Returns the cleaned dataset and prints the final shape.

### Part B: Save cleaned dataset
- Saves `clean_df` to `cleaned_rossmann_data.csv` in the current working directory.
- Prints a confirmation message after saving.

### Part C: `advanced_preprocessing(df)`
- Caps outlier `Sales` values using the IQR method to reduce extreme influence.
- Standardizes numeric features using `StandardScaler`:
  - `CompetitionDistance`
  - `Sales_Lag_7`
  - `Sales_Lag_30`
- Returns the scaled dataset as `scaled_df`.

## Cell 7 (Markdown) - EDA overview
- Introduces the EDA section and highlights the focus on business-relevant patterns like seasonality, promotion effects, and correlations.

## Cell 8 (Code) - EDA visualization suite
- Defines `generate_comprehensive_eda(df)` to create 8 charts and save them to a `visualizations/` folder:
  1. Sales distribution histogram with KDE.
  2. Boxplot comparing promo vs non-promo sales.
  3. Line plot of average monthly sales.
  4. Bar plot of average sales by day of week.
  5. Violin plot of sales across store types.
  6. Bar plot of average sales by assortment level.
  7. Correlation heatmap of selected features.
  8. Scatter plot of competition distance vs sales (sampled).
- Creates the `visualizations/` directory if missing.
- Calls the function on `scaled_df` and prints a success message.
- Note: Output location depends on the current working directory. If you run the notebook from `notebooks/`, the images land in `notebooks/visualizations/`.

## Cell 9 (Markdown) - Modeling and validation strategy
- Explains why random splits are invalid for time-series data.
- Introduces expanding-window validation with `TimeSeriesSplit`.
- Names the two models and the evaluation metrics (RMSE, MAE).

## Cell 10 (Code) - Model training and evaluation
- Defines `train_and_evaluate(df)` to:
  - Sample 250,000 rows for faster training.
  - Split into features `X` and target `y` (Sales).
  - Create a 3-fold expanding-window split.
  - Train a Linear Regression baseline and a Random Forest model.
  - Compute RMSE and MAE for each fold.
  - Print average metrics for both models.
- Returns the trained Random Forest model as `trained_rf_model`.

## Cell 11 (Markdown) - Model interpretation and export
- States the intent to analyze feature importance and save the final model for deployment or submission.

## Cell 12 (Code) - Feature importance and model export
- `plot_feature_importance(model, feature_names)`:
  - Extracts `feature_importances_` from the Random Forest.
  - Sorts and plots them as a horizontal bar chart.
  - Saves the figure to `visualizations/9_feature_importance.png`.
- `export_model(model, folder_path='models/')`:
  - Creates the output directory if missing.
  - Saves the model as `retail_rf_model.joblib` using `joblib`.
- Builds the feature list from `scaled_df` and executes both functions.
- Note: Output directories are relative to the current working directory. If run from `notebooks/`, the outputs go to `notebooks/visualizations/` and `notebooks/models/`.
