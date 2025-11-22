# Dogecoin Price Forecasting & Direction Classification  
A concise and professional overview of a small Jupyter-based project focused on **forecasting Dogecoin’s future price (regression)** and **predicting its future direction (classification)** using machine learning, technical indicators, and return-based modeling.

---

## 📌 Project Overview

This notebook explores whether historical OHLCV data and technical indicators can provide predictive power for Dogecoin (DOGE).  
The work is split into **two parts**:

1. **Regression** — Forecasting *log returns* of Dogecoin (daily dataset).  
2. **Classification** — Predicting whether Dogecoin will move **up** or **down** in the next candle (H1 dataset).

The project focuses on building features, applying market-theory justification, training Random Forest models, and visualizing predictions.

---

## 📂 Data Sources

- **DOGE-USD daily historical data** (`DOGE-USD.csv`)  
- **DOGEUSDT hourly data** (`DOGEUSDT_H1.csv`)

Both datasets contain standard OHLCV fields; volume is omitted from the H1 dataset.

---

## 📈 Market Theory Foundation

### Efficient Market Hypothesis (EMH)
The project acknowledges EMH, which states that:

- Prices reflect all public information.
- Predicting future price direction from past prices alone should be statistically impossible in an *efficient* market.
- Any consistent ability to forecast returns implies exploitable inefficiencies.

**Why we use log returns:**  
Prices are non-stationary; log returns help stabilize variance, improve stationarity, and make the series more suitable for statistical learning.

---

## 🧮 Feature Engineering

The notebook constructs an extensive feature set combining **technical indicators + price action**:

### Technical Indicators (via `ta`)
- **EMA 9, 20, 50**
- **Aroon indicator**
- **ADX**
- **CCI**
- **RSI**
- **ROC (Rate of Change)**
- **ATR (Average True Range)**
- **Bollinger Band width**
- **BB upper/lower band indicators**
- **VWAP** (if volume available)

### Price Action Features
- Candle body size  
- Avg candle size (rolling)  
- Wick size & wick/body ratio  
- Momentum flag (body ≥ wick)  
- Custom Bollinger segments (upper/lower 40%)  
- Moving averages for visualization  
- Custom signal plotting function with candlesticks

All infinite values are replaced, and missing rows are dropped for clean modeling.

---

## 🧠 Part A — Regression  
### Goal  
Predict the **next log return** of Dogecoin using a **RandomForestRegressor**.

### Workflow
- Target: `Log_Returns` shifted by +1 day  
- 80/20 chronological train-test split  
- Model: `RandomForestRegressor(n_estimators=1000)`
- Evaluation: MSE & RMSE

### Result
RMSE ≈ **0.044**  
This indicates a modest ability to approximate return volatility.

### Visualization
The notebook plots:
- Actual vs. predicted log returns  
- Zoomable start/stop ranges for clean inspection

---

## 🧠 Part B — Direction Classification  
### Goal  
Determine whether the next 1-hour candle will close **up (1)** or **down (0)**.

### Workflow
- Computes H1 log returns  
- Converts future shifted return into an **up/down label**  
- Feature engineering applied identically as in Part A  
- 80/20 time-based split  
- Model: `RandomForestClassifier(n_estimators=100)`

### Result
Accuracy generated via:
`accuracy_score(y_test, y_pred)`  

---

## 📊 Visualization Tools

- **Matplotlib**: Time-series plotting  
- **Plotly Candlestick Chart**:  
  - Bollinger Bands  
  - Moving averages  
  - 40% price segments  
  - Bullish/Bearish signal markers  
  - Fully customized black theme for clarity  

---

## 🧪 Key Insights

- Log-return regression captures short-term volatility but remains noisy and difficult to predict precisely (expected due to EMH).  
- Direction classification offers a more stable approach for short-horizon trading.  
- Technical indicators significantly improve representation vs raw prices.  
- Time-based splitting avoids look-ahead bias.  

---

## ⚠️ Disclaimer

This project is **strictly educational**.  
Cryptocurrency markets are volatile, and no model here should be used for financial decisions or trading.  
Machine-learning forecasts on OHLCV data are **not reliable** for real-world trading without extensive validation, risk management, and live execution testing.  
Use entirely at your own risk.

---

## 📄 Summary

This notebook demonstrates:

- Data preprocessing & visualization  
- Feature engineering (technical + price action)  
- Regression of log returns  
- Binary classification of direction  
- Evaluation through RMSE and accuracy  

A clean, concise exploration of applying machine learning to crypto forecasting within market-efficiency constraints.

