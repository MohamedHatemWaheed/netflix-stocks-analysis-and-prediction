# netflix-stocks-analysis-and-prediction
# 🎬 Netflix Stock Analysis (2020 – Present)

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=for-the-badge&logo=python&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical-008080?style=for-the-badge&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)

> **Financial data analysis project tracking Netflix Inc. (NASDAQ: NFLX) stock performance from 2020 to the present — combining Python-powered data engineering, technical indicators, and an interactive BI dashboard.**

---

## 📊 Dashboard Preview

![Netflix Stock Dashboard](Netflix%20Financial%20and%20Stocks%20Analysis.png)

> Dashboard built in Power BI featuring KPI cards, price trend analysis, RSI indicator, volume trends, and year-over-year performance.

---

## 📌 Key Metrics

| Metric | Value | What It Means |
|---|---|---|
| 💰 Price Change (2020–Present) | **$185.83** | Net cumulative dollar gain since the base period |
| 📈 Current RSI (14-period) | **44.92** | Neutral zone — not overbought, not oversold |
| 🔝 Highest Closing Price (5 Years) | **$1,340** | All-time 5-year peak — reached in the 2025 bull run |
| 🔻 Lowest Closing Price (5 Years) | **$166.37** | Maximum drawdown floor — hit during the 2022 crash |

---

## 🎯 Project Objectives

- Analyze Netflix stock price behavior across major market cycles (2020–2025)
- Engineer technical indicators (RSI, rolling averages) from raw OHLCV data
- Identify trend shifts, volume patterns, and momentum signals
- Build an interactive dashboard that communicates insights to both technical and non-technical audiences
- Derive data-driven investment insights with a financial storytelling lens

---

## 🗂️ Project Structure

```
netflix-stock-analysis/
│
├── netflix_stock_analysis.ipynb   # Main Google Colab notebook
├── data/
│   └── NFLX_historical_data.csv   # Raw OHLCV dataset (2020–2025)
├── dashboard/
│   └── Netflix_Dashboard.pbix     # Power BI dashboard file
├── visuals/
│   └── dashboard_preview.png      # Dashboard screenshot
└── README.md
```

---

## 🛠️ Tools & Technologies

| Tool | Role |
|---|---|
| **Python 3.x** | Core language for all analysis and processing |
| **Pandas** | Data loading, cleaning, manipulation, and aggregation |
| **NumPy** | Vectorised mathematical operations and rolling calculations |
| **Matplotlib** | Supplementary static visualisation during EDA |
| **yFinance** | Historical OHLCV data retrieval from Yahoo Finance |
| **Google Colab** | Cloud notebook environment for execution and sharing |
| **Power BI** | Final interactive dashboard, KPI cards, and slicers |

---

## ⚙️ Technical Steps

### 1. Data Acquisition
- Retrieved Netflix daily OHLCV data (2020–2025) via `yfinance` API
- Dataset fields: `Date`, `Open`, `High`, `Low`, `Close`, `Volume`, `Adj Close`

```python
import yfinance as yf

nflx = yf.download("NFLX", start="2020-01-01", end="2025-12-31")
nflx.to_csv("NFLX_historical_data.csv")
```

### 2. Data Cleaning & Preprocessing
- Set `Date` as a `DatetimeIndex` for time-series operations
- Forward-filled missing values for non-trading days (`ffill`)
- Cast `Close` and `Volume` to `float64` to prevent type errors
- Sorted chronologically and reset index

```python
import pandas as pd

df = pd.read_csv("NFLX_historical_data.csv", parse_dates=["Date"], index_col="Date")
df.sort_index(inplace=True)
df.ffill(inplace=True)
```

### 3. Feature Engineering

**RSI (Relative Strength Index — 14-period Wilder Method):**

```python
import numpy as np

def compute_rsi(series, period=14):
    delta = series.diff()
    gain = delta.clip(lower=0)
    loss = -delta.clip(upper=0)
    avg_gain = gain.ewm(alpha=1/period, min_periods=period).mean()
    avg_loss = loss.ewm(alpha=1/period, min_periods=period).mean()
    rs = avg_gain / avg_loss
    return 100 - (100 / (1 + rs))

df["RSI"] = compute_rsi(df["Close"])
```

**Additional features engineered:**
- `Year`, `Month` columns for temporal aggregations
- 28-day rolling close price for recent movement panel
- `Price_Change` = current close − earliest close

### 4. Exploratory Data Analysis (EDA)

```python
import plotly.express as px

# Average closing price by year
yearly_avg = df.groupby("Year")["Close"].mean().reset_index()
fig = px.bar(yearly_avg, x="Year", y="Close", title="Average Closing Price by Year")
fig.show()

# Close vs Volume scatter
fig2 = px.scatter(df, x="Volume", y="Close", title="Sum of Close by Volume")
fig2.show()
```

### 5. Dashboard Assembly (Power BI)
- Imported cleaned CSV into Power BI
- Built KPI cards with conditional formatting for all 4 headline metrics
- Created 6 chart panels with interactive year / month / day slicers
- Applied Netflix dark theme (black background, red accents)

---

## 📈 Results — Year-over-Year Performance

| Year | Avg. Closing Price | Context |
|---|---|---|
| 2020 | **$447** | Pandemic-driven subscriber boom |
| 2021 | **$558** | Peak growth euphoria |
| 2022 | **$285** | Brutal reset — first subscriber loss, −49% YoY |
| 2023 | **$390** | Recovery and margin stabilisation |
| 2024 | **$672** | Re-acceleration, ad-tier traction |
| 2025 | **$1,076** | Structural breakout — new all-time highs |

---

## 📖 The Story Behind the Data

### 🦠 Act I — The Pandemic Windfall (2020–2021)
When global lockdowns hit in March 2020, Netflix became essential infrastructure. Subscriber numbers exploded, institutions piled in, and the stock averaged **$447** in 2020 then **$558** in 2021. Volume was elevated — a classic sign of institutional accumulation.

### 💥 Act II — The Great Reset (2022)
The reckoning arrived in Q1 2022: Netflix reported its first subscriber loss in over a decade. The market responded brutally. The stock collapsed to an average of **$285**, touching the 5-year low of **$166.37**. Volume surged again — but this time it was fear-driven distribution. The stock lost nearly **49% year-over-year**.

### 🔄 Act III — The Quiet Rebuild (2023)
Netflix stopped apologising and started executing — password-sharing crackdowns, ad-supported tiers, and improved content margins rebuilt confidence. The stock recovered to an average of **$390**. RSI stabilised in neutral territory. Not exciting, but directionally sound.

### 🚀 Act IV — The Re-Rating (2024–2025)
Netflix transformed from a growth story into a **free-cash-flow story** and the market re-rated it accordingly. Average close reached **$672** in 2024 and **$1,076** in 2025, with an all-time closing high of **$1,340**. The RSI at **44.92** shows the stock has taken a healthy breath — pulling back from overbought levels without entering distress territory.

---

## 💡 Insights

- 📉 **Declining volume on rising price (2023–2025)** suggests early speculative sellers have been exhausted, leaving a more stable institutional holder base
- 📊 **RSI at 44.92** places Netflix near a historically favorable re-entry zone for medium-term investors — below 50 but not yet oversold
- 🔗 **The Close vs. Volume scatter** shows highest price density at moderate volume levels — a hallmark of controlled, institutional-led price appreciation rather than retail speculation
- 📅 **2022 is the single most important year** in this dataset: it quantifies the market's sensitivity to subscriber growth deceleration and sets the long-term risk floor at $166.37

---

## ✅ Recommendations

### For Investors
- Watch for RSI entries in the **35–40 band** — historically a higher-probability zone for medium-term positions in structurally sound growth names
- Use **$166.37** as a catastrophic downside reference for position sizing — the reward-to-risk ratio at current prices remains compelling
- Wait for **volume-confirmed bounces** off the 28-day moving average before adding new exposure at elevated prices


*Made with ❤️ and Python*
