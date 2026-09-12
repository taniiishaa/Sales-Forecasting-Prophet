# 📈 Sales Forecasting with Prophet

> **From thousands of individual transactions to a calendar of expected sales.**

Sales data is rarely just a collection of numbers.
It has **time, trends, seasonality, gaps, peaks, and changing patterns**.

This project explores historical sales data and turns it into a **daily time series**, then uses **Facebook Prophet** to forecast future sales.

The goal is simple:

**Understand the past → model the pattern → estimate the future.**

---

## 🔭 What This Project Does

The project takes transaction-level sales data and builds a complete forecasting workflow:

```text
Raw Transaction Data
        │
        ▼
   Data Cleaning
        │
        ▼
 Daily Sales Aggregation
        │
        ▼
 Missing / Zero-Day Handling
        │
        ▼
 Time-Series Exploration
        │
        ▼
 Chronological Train/Test Split
        │
        ▼
   Prophet Forecasting
        │
        ▼
   Future Predictions
        │
        ▼
      RMSE
        │
        ▼
 Forecast Visualization
```

Rather than randomly splitting the dataset like a typical machine-learning problem, the project respects the **timeline** — training on the past and evaluating against a later period.

---

## 🧠 The Core Idea

A forecasting problem asks a different question from ordinary classification or regression.

Instead of:

> *“What category does this record belong to?”*

we ask:

> **“Given everything that happened before, what might happen next?”**

For this project, the target is:

**Daily Sales**

The historical transactions are transformed into a chronological sales series before being passed to Prophet.

---

## 🗓️ From Transactions → Time Series

The original dataset contains individual orders.

Those transactions are grouped by date:

```text
Individual Orders
────────────────────────────
Order 1 → ₹...
Order 2 → ₹...
Order 3 → ₹...
Order 4 → ₹...
        ↓
      Group by Date
        ↓
────────────────────────────
2014-01-01 → Daily Sales
2014-01-02 → Daily Sales
2014-01-03 → Daily Sales
...
```

This produces a cleaner time series that Prophet can model.

---

## 🧹 Handling Zero-Sales Days

Real-world datasets aren't always perfectly continuous.

The project identifies days where aggregated sales are zero. These gaps are treated as missing observations and forward-filled before modelling.

```text
Raw Daily Series

Sales
 │
 │       █
 │   █   █
 │   █   █       █
 │   █   ░   █   █
 │___█___░___█___█______ Time

        ░
     zero-sales day
        ↓
   treated as missing
        ↓
   forward-filled
```

This preprocessing step helps create a continuous series for forecasting.

---

## 📊 Exploratory View

Before forecasting, the project visualizes daily sales to understand how the series behaves over time.

This helps reveal:

* Long-term movement
* Sales fluctuations
* Seasonal patterns
* High and low periods
* Irregular behaviour

The visualization is important because forecasting should not be treated as a black-box operation.

---

## ⏳ Time-Aware Validation

One of the most important design decisions in this project is the **chronological train/test split**.

### Training Period

**2014 → 2016**

### Testing Period

**2017**

```text
TIME ───────────────────────────────────────────────►

2014          2015          2016          2017
│────────────── TRAINING ──────────────│──── TEST ────│
                                      ↑
                              Forecast starts here
```

This mirrors the real forecasting scenario:

> Use historical data to predict a future period.

No future observations are allowed to leak into training.

---

## 🔮 Why Prophet?

The project uses **Facebook Prophet**, a forecasting framework designed around time-series patterns such as:

* Trend
* Seasonality
* Changepoints
* Historical time-dependent behaviour

The model configuration used here includes:

```python
Prophet(
    yearly_seasonality=True,
    changepoint_prior_scale=0.1
)
```

The model is trained on the historical daily sales series and then used to generate predictions for the test period.

---

## ⚙️ Forecasting Pipeline

```text
                HISTORICAL SALES
                       │
                       ▼
              Prepare ds / y
                       │
                       ▼
              ┌─────────────────┐
              │     PROPHET     │
              │                 │
              │ Trend           │
              │ Seasonality     │
              │ Changepoints    │
              └────────┬────────┘
                       │
                       ▼
               Future DataFrame
                       │
                       ▼
                  Predictions
                       │
                       ▼
             Clip Negative Values
                       │
                       ▼
                 RMSE Evaluation
```

---

## 🧪 Model Evaluation

Forecast quality is evaluated using **Root Mean Squared Error (RMSE)**.

### RMSE

$$
RMSE =
\sqrt{
\frac{1}{n}
\sum_{i=1}^{n}
(y_i-\hat{y_i})^2
}
$$

Where:

* `yᵢ` = actual sales
* `ŷᵢ` = predicted sales
* `n` = number of observations

A lower RMSE indicates that predictions are, on average, closer to the actual sales values.

The notebook calculates RMSE on the held-out **2017 test period** rather than reporting a placeholder score.

---

## 📉 Actual vs Forecast

The project compares:

```text
Actual Sales
     │
     │    ╭──╮
     │ ╭──╯  ╰──╮
     │─╯        ╰──────
     │
     │    ┄┄┄┄┄┄┄┄┄
     │      Forecast
     └──────────────────► Time
```

The notebook generates an actual-vs-predicted visualization with the resulting RMSE included in the chart title.

This provides both:

**Numerical evaluation + visual evaluation**

---

## 📦 Dataset → Forecast Output

The workflow also produces a forecast submission containing:

* Forecast IDs
* Predicted sales
* A total of **364 forecast entries**

The notebook assigns IDs beginning from `3000888` when creating the submission output.

---

## 🗂️ Project Structure

```text
sales-forecasting-prophet/
│
├── 📓 Untitled.ipynb
│
├── 📊 stores_sales_forecasting.xls
│
├── 📊 final_sales_forecast.xls
│
└── 📄 README.md
```

> **Note:** The notebook currently references CSV input/output filenames in parts of the workflow, while the repository contains `.xls` files. Renaming the notebook and aligning the dataset/output extensions would be a worthwhile cleanup before treating this as a polished portfolio repository.

---

## 🛠️ Technology Stack

| Technology                 | Purpose                     |
| -------------------------- | --------------------------- |
| 🐍 Python                  | Core implementation         |
| 🐼 Pandas                  | Data loading & manipulation |
| 🔢 NumPy                   | Numerical operations        |
| 📈 Matplotlib              | Visualization               |
| 🎨 Seaborn                 | Exploratory visualization   |
| 🔮 Prophet                 | Time-series forecasting     |
| 📐 Scikit-style evaluation | RMSE calculation            |

---

## 🚀 Running the Project

### 1. Clone the repository

```bash
git clone https://github.com/taniiishaa/sales-forecasting-prophet.git
cd sales-forecasting-prophet
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn prophet scikit-learn
```

### 3. Launch Jupyter Notebook

```bash
jupyter notebook
```

Open the forecasting notebook and execute the cells sequentially.

---

## 🧩 What I Learned

This project strengthened my understanding of how forecasting differs from conventional machine learning.

### Key takeaways

* Time series must preserve chronological order.
* Random train/test splitting can cause future-data leakage.
* Raw transaction data often needs to be aggregated before modelling.
* Missing or zero-value periods require thoughtful preprocessing.
* Forecasting models should be evaluated on genuinely unseen future periods.
* A model's predictions should be inspected visually, not only through one metric.

---

## 🌱 Possible Next Steps

This project creates a solid baseline for a more advanced forecasting system.

Potential improvements include:

* [ ] Rename and clean the notebook
* [ ] Align dataset filenames and formats
* [ ] Add automated data validation
* [ ] Compare Prophet with ARIMA/SARIMA
* [ ] Experiment with XGBoost-based forecasting
* [ ] Add additional regressors
* [ ] Perform rolling/expanding-window validation
* [ ] Add confidence intervals and forecast uncertainty
* [ ] Build an interactive Streamlit forecasting dashboard
* [ ] Export forecasts automatically
* [ ] Deploy the forecasting application

---

## 🎯 Project Perspective

This project sits at an interesting intersection of:

**Data Analysis + Time-Series Modelling + Business Forecasting**

A sales forecast can ultimately support questions such as:

```text
"What might sales look like next month?"
                    │
                    ▼
        ┌─────────────────────┐
        │  Forecasting Model  │
        └──────────┬──────────┘
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    Inventory   Planning   Strategy
```

The notebook is therefore more than a model experiment — it demonstrates the complete journey from **historical business data to a future-oriented prediction**.

---

## ⭐ Final Takeaway

> **The past gives us patterns. Forecasting turns those patterns into possibilities.**

This project demonstrates a practical approach to transforming transaction-level sales data into a structured daily time series, modelling its temporal behaviour with Prophet, and evaluating predictions against a genuinely unseen future period.

**Built as a hands-on exploration of time-series forecasting with Python.**
