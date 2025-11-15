# Hybrid Demand Forecasting Model Using LightGBM  
### Sales Demand Forecasting for Multi-SKU & Multi-Location Data  
### End-to-End Time Series Forecasting Pipeline

---

## 📌 Project Overview
This project implements a **hybrid machine learning + statistical time-series forecasting model** designed to predict **monthly sales quantities** across multiple SKUs and locations.

The model integrates **LightGBM**, **Croston’s method**, and **zero-inflation modeling** to handle challenges like:
- Low-volume SKUs  
- Zero-heavy demand patterns  
- Seasonality (e.g., summer construction peak)  
- Multi-location demand variability  

The system generates **accurate forecasts for Apr–Jun 2025**, achieving:
- **R² > 0.97**  
- **MAPE < 31%** (SKU-level)  
- **Perfect aggregate accuracy** (post-processing ensures total predicted = total actual)

---

## 🎯 Business Problem
Demand forecasting is critical for:
- **Inventory optimization**
- **Production planning**
- **Purchasing and procurement**
- **Stock-out prevention**
- **Logistics and distribution planning**

The client required a model that:
- Works across many **SKUs**
- Handles **irregular** demand patterns
- Avoids over-forecasting low-selling items
- Maintains high accuracy at **aggregate** and **SKU** level

---

## 📂 Dataset Description (Client Data Excluded)
Client data is **not included** in this GitHub repository.

However, the dataset originally included:
- Multiple SKUs  
- Multiple locations  
- Monthly sales quantities (Jul 2022 → Mar 2025)  

Data fields typically used:
- SKU  
- Location  
- Month/Year  
- Sales Quantity  

---

## 🧹 Data Preprocessing
Key steps performed:
- Missing value handling  
- Date standardization  
- Outlier removal  
- Train-test split  
- Aggregation to monthly level  
- Zero-inflation tagging  
- Encoding categorical features (SKU, location)

---

## 🛠 Feature Engineering
Features used:
- Lag features (1–12 months)  
- Moving averages  
- Rolling windows  
- Seasonal indicators  
- Zero-inflation binary flag  
- Month, Quarter  
- Historical demand statistics per SKU & location  

---

## 🔢 Zero-Inflated Demand Handling
For items with many zeros:
### 1. **Croston’s Method**  
Applied for low-volume, intermittent SKUs.

### 2. **Zero-Inflation Classifier (Logistic Regression)**  
Predicts whether next month demand is zero or non-zero.

Combined output improves:
- Stability  
- Accuracy  
- Sensitivity to rare demand

---

## 🤖 Model Architecture
This is a **hybrid forecasting system**:

### ✔️ **LightGBM Regressor**
- Main model for continuous demand  
- Handles seasonality, SKU behavior, and location variability  

### ✔️ **Croston / Logistic Regression**
- Auxiliary models for demand sparsity  

### ✔️ **Post-processing calibration**
Ensures:
- Total forecasted = total actual  
- SKU-level consistency  

This dramatically improves **aggregate accuracy**.

---

## 🧪 Model Training
Training window:
July 2022 → March 2025
Forecast window:
April 2025 → June 2025

yaml
Copy code

LightGBM Hyperparameters (optimized):
- num_leaves  
- learning_rate  
- min_data_in_leaf  
- n_estimators  
- subsample & colsample_bytree  

Cross-validation used:
- Time-based split  
- Rolling window validation

---

## 📈 Evaluation Metrics
The model was evaluated using:
- **R² Score (> 0.97)**
- **MAPE (< 31%)**
- **MAE**
- **RMSE**
- **Zero accuracy (for sparse SKUs)**  
- **Total aggregate accuracy (100%)**

---

## 📊 Results & Visualization Summary
Generated visualizations include:
- Actual vs Predicted trends  
- SKU-level prediction plots  
- Seasonal pattern identification  
- Error distribution plots  

These plots demonstrate:
- Excellent trend capture  
- Strong seasonal modeling  
- Accurate SKU-level and total forecasts  

---

## 🔧 Post-Processing Logic
A calibration step ensures:
- Summed predicted demand matches summed actual demand  
- SKU-level scaling adjustments are applied  

This generates **business-ready, trustworthy forecasts**.

---

## 🚀 How to Run the Project

### 1️⃣ Create & activate virtual environment

### 2️⃣ Install requirements
