# 🌞 Solar Energy Prediction using Hybrid Model (XGBoost + LSTM)

## 📌 Overview

This project predicts solar power generation using a hybrid machine learning approach combining:

* **XGBoost (Gradient Boosting)** for tabular feature learning
* **LSTM (Neural Network)** for time-series pattern learning

The dataset is sourced from Kaggle:

* Solar generation data (15-minute intervals)
* Weather data (temperature, humidity, wind, etc.)
* Site metadata (capacity, panel details)

---

## ⚙️ Problem Formulation

We model this as a **regression problem**:

Input:

* Weather features
* Time-based features
* Historical solar values (lag features)

Output:

* Normalized solar generation

---

## 🔥 Key Engineering Decisions

### 1. Target Normalization

```text
target = SolarGeneration / kWp
```

✔ Removes site capacity bias
✔ Makes model generalizable across systems

---

### 2. Feature Engineering

#### Time-based features:

* Hour → sin/cos encoding
* Day of year → seasonal encoding
* Day/Night indicator

#### Lag features:

* lag_1, lag_2, lag_3, lag_6
* rolling mean & std

✔ Captures temporal dependency and recent trends

---

### 3. Models Used

#### 🔹 XGBoost

* Handles tabular data efficiently
* Captures spikes and non-linear relationships

#### 🔹 LSTM

* Captures sequential dependencies
* Uses 48 time steps (~12 hours history)

---

### 4. Hybrid Model

Final prediction:

```text
Final = 0.6 * XGBoost + 0.4 * LSTM
```

✔ Combines:

* Spike accuracy (XGB)
* Trend learning (LSTM)

---

## 📊 Results

### Final Performance:

* **RMSE:** 0.0266
* **MAE:** 0.0117

These values are on **normalized scale (0–1)**.

### Interpretation:

* Error ≈ **1–2% deviation**
* Very strong predictive performance

---

## 📈 Output Behavior

From the plotted graph:

* Model tracks **daily solar cycles accurately**
* Peaks are captured reasonably well
* Slight smoothing still exists (due to missing irradiance)

---

## ⚠️ Limitations

### ❌ Missing Solar Irradiance

The dataset does NOT include:

* GHI (Global Horizontal Irradiance)
* DNI (Direct Normal Irradiance)

👉 This is the most important physical variable for solar power.

Because of this:

* Model relies on indirect indicators (time + lag)
* Peak prediction is slightly limited

---

## 🚀 Future Improvements

To further improve accuracy:

### 1. Add Irradiance Data

Would drastically improve performance.

### 2. Hyperparameter Tuning

* Optuna / Grid Search

### 3. Advanced Models

* Transformer-based time series models
* Temporal Fusion Transformer (TFT)

### 4. Deployment

* Real-time prediction API
* Dashboard visualization (React)

---

## 🧠 Key Insight

Solar generation follows:

```text
Power ≈ Irradiance × Efficiency × Area
```

Since irradiance is missing:
👉 Model learns this indirectly using:

* Time cycles
* Weather
* Past generation

---

## ✅ Conclusion

This hybrid approach successfully:

* Combines ML + Deep Learning
* Handles time-series + tabular data
* Achieves high accuracy (~1–2% error)

👉 This is a **production-level approach** used in real-world energy forecasting systems.

---

## 👨‍💻 Tech Stack

* Python
* TensorFlow (LSTM)
* XGBoost
* Scikit-learn
* Pandas / NumPy
* Matplotlib

---

## ⭐ Final Note

Even without irradiance, achieving:

```text
RMSE ≈ 0.026
MAE ≈ 0.011
```

is **very strong performance** and indicates a well-designed pipeline.
