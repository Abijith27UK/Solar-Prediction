# Solar Energy Generation Prediction

This project predicts **solar power generation** using historical solar generation data, weather data, and site-level metadata.  
The model is built using a **Random Forest Regressor** with feature engineering techniques such as lag features, rolling averages, cyclical time encoding, and weather-derived variables.

---

## 📌 Project Overview

The goal of this project is to accurately forecast **SolarGeneration** using:

- Historical solar generation values
- Weather parameters
- Solar site details
- Time-based engineered features

This helps in:
- Solar energy forecasting
- Grid planning and management
- Renewable energy analytics
- Smart energy optimization

---

## 📂 Dataset Used

The project uses three datasets:

1. **Solar Energy Generation Data**
   - `CampusKey`
   - `SiteKey`
   - `Timestamp`
   - `SolarGeneration`

2. **Weather Data**
   - `CampusKey`
   - `Timestamp`
   - `ApparentTemperature`
   - `AirTemperature`
   - `DewPointTemperature`
   - `RelativeHumidity`
   - `WindSpeed`
   - `WindDirection`

3. **Solar Site Details**
   - `CampusKey`
   - `SiteKey`
   - `kWp`
   - `Number of panels`
   - `lat`
   - `Lon`

---

## ⚙️ Workflow

### 1. Data Loading
The three datasets are loaded using **Pandas**.

### 2. Data Preprocessing
- Converted `Timestamp` columns to datetime format
- Merged solar generation data with weather data using:
  - `CampusKey`
  - `Timestamp`
- Merged site details using:
  - `CampusKey`
  - `SiteKey`
- Removed duplicates
- Sorted data by `SiteKey` and `Timestamp`

### 3. Feature Engineering

#### ⏰ Time-Based Features
- `hour`
- `day`
- `month`
- `dayofweek`

#### 🔁 Cyclical Encoding
To preserve the circular nature of time:
- `hour_sin`, `hour_cos`
- `month_sin`, `month_cos`

#### 🌦 Weather-Derived Features
- `temp_spread = AirTemperature - DewPointTemperature`
- `wind_humidity_interaction = WindSpeed × RelativeHumidity`

#### 🌬 Wind Direction Encoding
Instead of using raw wind direction angle:
- `wind_dir_sin`
- `wind_dir_cos`

#### 📈 Lag Features
Historical values were included to improve predictive performance:
- `SolarGeneration_lag1`
- `SolarGeneration_lag2`
- `AirTemperature_lag1`

#### 📊 Rolling Feature
- `SolarGeneration_roll3` → 3-step rolling average of solar generation per site

### 4. Missing Value Handling
Numeric columns were interpolated and filled using:
- interpolation
- forward fill
- backward fill

Rows with `NaN` values caused by lag and rolling features were removed.

### 5. Train-Test Split
A **time-based split** was used:
- **80%** training data
- **20%** testing data

This preserves temporal order and avoids data leakage from future observations.

### 6. Model Training
Model used:
- **RandomForestRegressor**
  - `n_estimators = 150`
  - `max_depth = 15`
  - `random_state = 42`
  - `n_jobs = -1`

---

## 📊 Model Performance

The model achieved the following results on the test set:

- **MAE:** `0.1014`
- **RMSE:** `0.2977`
- **R² Score:** `0.9982`

### Interpretation
- **Low MAE and RMSE** indicate very small prediction error.
- **R² = 0.9982** shows the model explains almost all variance in solar generation.

> ⚠️ Note: Since lag and rolling features of the target (`SolarGeneration`) are used, the very high R² is expected.  
> This makes the model strong for **short-term forecasting**, but performance should also be validated carefully for real-world deployment to avoid overestimating generalization.

---

## 🔍 Top Feature Importances

| Rank | Feature | Importance |
|------|---------|------------|
| 1 | SolarGeneration_roll3 | 0.969058 |
| 2 | SolarGeneration_lag2 | 0.018445 |
| 3 | SolarGeneration_lag1 | 0.010929 |
| 4 | hour | 0.000142 |
| 5 | hour_sin | 0.000135 |
| 6 | wind_dir_cos | 0.000116 |
| 7 | wind_dir_sin | 0.000115 |
| 8 | DewPointTemperature | 0.000106 |
| 9 | wind_humidity_interaction | 0.000102 |
| 10 | day | 0.000098 |
| 11 | WindSpeed | 0.000095 |
| 12 | RelativeHumidity | 0.000079 |
| 13 | temp_spread | 0.000075 |
| 14 | ApparentTemperature | 0.000072 |
| 15 | AirTemperature_lag1 | 0.000066 |

### Key Insight
The model relies heavily on:
- **Rolling average of recent solar generation**
- **Lagged solar generation values**

This indicates that **recent generation history is the strongest predictor** of near-future solar output.

---

## 🛠 Tech Stack

- **Python**
- **Pandas**
- **NumPy**
- **Scikit-learn**
- **Random Forest Regressor**

---