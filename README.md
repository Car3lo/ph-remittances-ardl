# ARDL Modeling of Philippine Remittances

![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)
![Econometrics](https://img.shields.io/badge/Econometrics-ARDL-orange)

## Project Overview
This project performs a time-series analysis of cash remittances to the Philippines from Q1 2014 to Q4 2024. Using an Autoregressive Distributed Lag (ARDL) model, it investigates the determinants of remittance inflows, specifically testing the "Insurance Hypothesis"—whether migrants send more money in response to natural disasters (typhoons) and domestic unemployment.

## Repository Structure
* `analysis.Rmd`: The main RMarkdown file containing the data processing, modeling, and diagnostics.
* `data/`: Contains the cleaned quarterly dataset (`remittances_data.xlsx`).
* `output/`: Generated reports and figures.

## Key Findings
* **Typhoons:** A statistically significant immediate positive effect on remittances, supporting the role of remittances as a disaster "safety net."
* **Unemployment:** A significant positive response to lagged unemployment, suggesting migrants react to economic distress with a one-quarter delay.
* **Seasonality:** Strong seasonal spikes observed in Q3.

## Econometric Implementation

### 1. ARDL Model Estimation
To capture both short-run shocks and long-run equilibrium, I utilized the `ARDL` package. The final model specification (Model V3) uses 2 lags for remittances to correct for serial correlation, while assigning specific lag structures to economic variables based on theoretical reaction times.

```r
# Final Model Specification: ARDL(2, 1, 1, 1, 1, 0, 0, 0)
# Remittances are lagged by 2 quarters; Economic vars by 1 quarter.
manual_lags_v3 <- c(2, 1, 1, 1, 1, 0, 0, 0)

best_model_v3 <- ardl(
  ln_remit ~ typhoon + ggrowth + unem + inf + q2_dummy + q3_dummy + q4_dummy,
  data = ts_final, 
  order = manual_lags_v3
)

summary(best_model_v3)