Project overview
✔ Features
✔ Full pipeline explanation
✔ Time-series modeling details
✔ How to run
✔ Output description
✔ Plot sample section
✔ Folder structure
✔ Future improvements

You can copy–paste this directly into README.md.

📘 README.md — Time-Series Sales Forecasting Pipeline (LightGBM + Hybrid Model)
# 📦 Sales Demand Forecasting (Time-Series + ML Hybrid Model)

This repository implements a **hybrid time-series forecasting pipeline** that predicts 
monthly sales quantities for each **SKU × Location** combination using:

- 🟢 **LightGBM Regression**
- 🔵 **Time-series features (lags, rolling averages, EMA, WMA, seasonality, trend)**
- 🟡 **Intermittent-demand models (Croston-style adjustments)**
- 🟣 **Calibrated aggregation (ensures predicted totals match actual monthly totals)**

The model predicts demand for **April, May, and June 2025** and produces:
- Row-level predictions  
- Error metrics  
- Aggregated metrics  
- Actual vs Predicted comparison plot  

---

## 📊 **Modeling Approach**

This project uses a **Machine Learning Time-Series Forecasting (MLTSF)** approach.

Although the model uses LightGBM, it behaves as a **global time-series forecaster** because it incorporates:

### ✔ Lag features  


lag_1, lag_2, ..., lag_6
lag_12, lag_24
lag_diff_*


### ✔ Rolling statistics  


rolling_mean_3, rolling_mean_6, rolling_mean_12, rolling_mean_24
rolling_std_3, rolling_std_6


### ✔ Exponential moving averages  


ema_3, ema_6, ema_12


### ✔ Temporal & calendar features  


year, month_num, quarter, days_in_month
season, seasonal_strength, trend


### ✔ Intermittent demand enhancements  
- Zero-demand classifier (Logistic Regression)  
- Croston-style smoothing  
- Low/very-low volume fallback model  

### ✔ Monthly calibration  
Ensures predicted totals **match actuals at the month level** (zero aggregate error).

### 🌤️ Seasonal Features (Based on US Seasons)

The pipeline includes **US-based seasonal mapping** to capture broad seasonal demand patterns.

We assign each month to one of the four seasons:

| Season  | Months          |
|---------|-----------------|
| Winter  | December–February |
| Spring  | March–April     |
| Summer  | May–August      |
| Fall    | September–November |

Each record is mapped to its season and then converted into **one-hot encoded features**:

season_Winter
season_Spring
season_Summer
season_Fall

kotlin
Copy code

Example logic used in the code:

```python
def assign_season(month):
    if month in [12, 1, 2]: 
        return 'Winter'
    elif month in [3, 4]: 
        return 'Spring'
    elif month in [5, 6, 7, 8]: 
        return 'Summer'
    else: 
        return 'Fall'
These features help the model learn:

Holiday season demand patterns

Summer construction spikes

Spring recovery patterns

Low-demand cold-season effects

📌 Seasonal Calibration Factors
In addition to season classification, the model applies monthly seasonal calibration multipliers to correct aggregated forecasts:

python
Copy code
df['seasonal_factor'] = 1.0
df.loc[df['month_num'] == 4, 'seasonal_factor'] = 0.998
df.loc[df['month_num'] == 5, 'seasonal_factor'] = 1.048
df.loc[df['month_num'] == 6, 'seasonal_factor'] = 1.038
These adjustments fine-tune the model for months where seasonal patterns are strong.


Copy code

---

# 🔥 If you want, I can also add:

✅ A flowchart image showing how seasonal features are created  
✅ README diagram of the full pipeline  
✅ Seasonal demand plots  
✅ Example seasonal feature table  

Just tell me **“Add diagram”** or **“Add example table”**.

---

## 📁 **Project Structure**



├── data/
│ └── fuzi_sales_data_aggregated.csv
├── src/
│ ├── main.py
│ ├── feature_engineering.py
│ ├── model_training.py
│ ├── postprocessing.py
│ └── visualization.py
├── README.md
└── requirements.txt


---

## 🚀 **How to Run**

### **1. Install dependencies**
```bash
pip install -r requirements.txt

2. Run forecasting
python main.py

3. Output files

The script produces:

Per-month predictions (April / May / June)

Error metrics (MAE, RMSE, MAPE, WMAPE, R²)

Monthly summary table

Actual vs Predicted plot

📈 Visualization

The project includes a clean comparison plot:

Actual Qty → solid green line

Predicted Qty → dashed orange line

def simple_actual_vs_predicted_plot(monthly_outputs):
    ...


The plot looks like:

Actual: ────────────
Predicted: - - - - - - -

📑 Key Functions
Data Loading & Cleaning

Removes duplicates

Handles missing values

Handlig negative qty as 0

Restricts data to July 2022 → June 2025

Splits train/test logically by date

Feature Engineering

Adds time-series transformations

Calculates seasonality & trend

Creates SKU×Location history statistics

Applies lagging & rolling windows

Model Training

LightGBM with categorical SKU/Location

TimeSeriesSplit cross-validation

Low-volume fallback models

Post-Processing

Low-volume demand correction

Croston-style adjustments

Monthly calibration

Scaling predictions to match total actuals

Evaluation

Full set of error metrics

Segmented error breakdown

Worst-case prediction analysis

📊 Outputs

Each predicted month returns a DataFrame:

MONTH_START_DATE	MASTER_SKU_CODE	LOCATION	ACTUAL_QTY	PREDICTED_QTY	ERROR_%

A final summary prints:

=== FINAL CALIBRATED SUMMARY ===
2025-04-01 | Actual: XXX | Predicted: XXX | Error: 0.00%
2025-05-01 | Actual: XXX | Predicted: XXX | Error: 0.00%
2025-06-01 | Actual: XXX | Predicted: XXX | Error: 0.00%

<img width="1231" height="733" alt="image" src="https://github.com/user-attachments/assets/6a285d17-bf5a-4f64-a994-8d0da07b4c90" />


🔮 Future Improvements

Add Prophet / ARIMA / SARIMA models for comparison

Train N-BEATS / TFT neural network forecaster

Create interactive dashboards (Plotly)

Add hyperparameter optimization (Optuna)

Convert pipeline into modular classes

🧑‍💻 Author

Rev
AI / ML / Data Science

⭐️ Support

If you find this useful, please ⭐ the repository!


---

# Want your README to include?  
✔ Architecture diagram  
✔ Output sample images  
✔ Pipeline flowchart  

I can generate those too — just tell me!
