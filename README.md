# Forecasting DAX Volatility through Macroeconomic Drivers

MBA (Business Analytics) capstone project at the Symbiosis School for Online and Digital Learning (SSODL), Symbiosis International University, under the guidance of Dr Anuradha Goswami.

📄 [Full report](CAPSTONE%20PROJECT.pdf)

---

## Research question

Between 2020 and 2025 the German stock market went through COVID-19, the Eurozone energy crisis and the ECB's tightening cycle. **What drives volatility in the DAX, and do macroeconomic fundamentals explain it better than purely statistical volatility models?**

## Data

Monthly data for 2020–2025. Quarterly GDP is aligned to a monthly frequency.

| Variable | Source | File |
|---|---|---|
| DAX volatility (from log returns) | Yahoo Finance | `DAX Volatality.csv` |
| Real GDP growth | Eurostat | `QUARTERLY REAL GDP Percentage change from Year Ago.csv` |
| Consumer price index (CPI) | Eurostat | `CPI Inflation.csv` |
| Unemployment rate | Eurostat | `Unemployment Rate.csv` |
| 10-year government bond yield | Bundesbank | `Interest Rates Long Term Government Bond Yields 10 Year.csv` |
| EUR/USD exchange rate | European Central Bank | `EUR to USD.csv` |
| ZEW investor sentiment | ZEW Mannheim | `ZEW Sentiment.csv` |

All seven series are merged into `merged_data.csv`.

## Method

1. **Preparation.** Merge the seven sources, align frequencies, compute DAX log returns and volatility, test for stationarity (Augmented Dickey-Fuller) and difference where needed.
2. **Statistical baselines.**
   - ARIMA for autocorrelation and trend
   - Holt-Winters exponential smoothing for trend and seasonality
   - GARCH for volatility clustering
3. **ARIMAX with exogenous variables** (called SARIMAX in the report). An ARIMA(1,0,1) model on differenced volatility, with the six macroeconomic indicators as regressors, to identify which fundamentals move volatility.
4. **Model checks.** RMSE for the baselines; AIC/BIC, Ljung-Box and Jarque-Bera for the model with macro regressors.

## Results

**Baseline forecasts**

| Model | RMSE | Strength | Weakness |
|---|---|---|---|
| ARIMA | **0.0160** | Stable, good short-term fit | Misses volatility shocks |
| GARCH | 0.0167 | Models volatility clustering | Too flat in extreme periods |
| Holt-Winters | 0.0179 | Smooth trend and seasonality | Handles sharp shocks poorly |

**Macroeconomic drivers of DAX volatility** (model with macro regressors; standardised coefficients as reported)

| Variable | Coefficient | p-value | Significant? |
|---|---|---|---|
| CPI | −0.0228 | < 0.001 | ✅ |
| GDP growth | −0.0109 | 0.045 | ✅ |
| Unemployment | +0.0121 | 0.023 | ✅ |
| 10-year bond yield | +0.0063 | 0.162 | — |
| EUR/USD | +0.0036 | 0.400 | — |
| ZEW sentiment | +0.0019 | 0.508 | — |

Model fit: AIC = −181.44, BIC = −167.42. Residual diagnostics show no remaining autocorrelation (Ljung-Box p = 0.90) and no departure from normality (Jarque-Bera p = 0.72).

**Takeaways**
- DAX volatility clusters: calm and turbulent periods persist.
- **Growth, inflation and the labour market** are the significant drivers. Stronger GDP growth goes with lower volatility, and rising unemployment with higher volatility. Higher CPI is associated with lower volatility in this period, possibly because inflation expectations were already priced in.
- Bond yields, the exchange rate and ZEW sentiment were not significant over 2020–2025.
- For risk management, unemployment data is a measurable leading signal of volatility.

**Limitations** (from the report): volatility is the only risk measure; macro data is monthly or quarterly rather than high-frequency; no machine-learning models; geopolitical shocks are reflected only indirectly; DAX only, with no cross-market spillovers.

## Repository contents

| File | Purpose |
|---|---|
| `Capstone Data Cleaning.ipynb` | Cleaning, frequency alignment and merging of the seven series |
| `Capstone EDA.ipynb` | Exploratory analysis of each macroeconomic series and of DAX volatility |
| `Capstone Advanced Analytics.ipynb` | ARIMA, Holt-Winters, GARCH and ARIMA with macroeconomic regressors |
| `*.csv` | Raw series and the merged dataset |
| `CAPSTONE PROJECT.pdf` | Full capstone report |

## How to run

```bash
pip install -r requirements.txt
jupyter notebook
```

Run the notebooks in the order listed above. They read their data from the repository root.

## Author

**Vishwas Khandelwal** · [GitHub](https://github.com/Vikh110) · [LinkedIn](https://www.linkedin.com/in/vishwas-k-07842a283/)
