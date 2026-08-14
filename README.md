# STAT0011 Reports

R Markdown reports for STAT0011 (Decision and Risk), covering time series and copula-based modelling of financial return data (S&P 500 and Nikkei 225), including ARMA-GARCH/TGARCH volatility modelling and copula fitting for portfolio risk analysis.

## Contents

- `group_33_report.Rmd` — Full group ICA report (Group 33).
- `STAT0011_ICA_Report_YB_s_section_updated2.Rmd` — Individual section contribution.

## Data

Market data is pulled live from Yahoo Finance via `quantmod::getSymbols()`; no local data files are required to reproduce the analysis.

## Requirements

R packages: `quantmod`, `fitdistrplus`, `fGarch`, `rugarch`, `VineCopula`, `KScorrect`, `ADGofTest`, `FinTS`, `tseries`, `knitr`, `plotly`.
