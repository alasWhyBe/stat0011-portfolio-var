# STAT0011 Portfolio VaR

## Problem

Quantify the 1-week downside risk of an equally-weighted portfolio of the S&P 500 and the Nikkei 225, using weekly log returns from 2000–2025 (1,235 observations). Risk is measured via Value-at-Risk (VaR) and Conditional Value-at-Risk (CVaR) at the 95% and 99% confidence levels, using a copula-based Monte Carlo approach that captures the joint (not just individual) tail behaviour of the two markets.

## Approach

1. **Marginal models** — For each index, selected an ARMA order via an AIC grid search over ARMA(0–5), then compared GARCH, TGARCH, and eGARCH volatility specifications against six candidate conditional distributions, selecting by AIC/BIC. TGARCH with a skewed Student-t distribution won for both indices:
   - S&P 500: ARMA(0,0)-TGARCH(1,1)
   - Nikkei 225: ARMA(3,0)-TGARCH(1,1)
2. **Diagnostics** — Validated standardised residuals with Ljung-Box (no remaining autocorrelation) and Engle's ARCH test (no remaining ARCH effects) for both models.
3. **Dependence structure** — Applied the Probability Integral Transform to the standardised residuals, then fit a copula to model the dependence between the two indices. A Student-t copula was selected by AIC (ρ = 0.55, df = 17.39, Kendall's τ = 0.37).
4. **Simulation** — Monte Carlo simulated 10,000 joint one-week-ahead return scenarios from the fitted copula and marginal models, then computed portfolio VaR and CVaR from the simulated distribution, benchmarked against historical (empirical) estimates.

## Results

| Confidence | Simulated VaR | Historical VaR | Simulated CVaR | Historical CVaR |
|---|---|---|---|---|
| 99% | -17.8% | -6.5% | -21.1% | -9.7% |
| 95% | -10.9% | -3.8% | -14.8% | -5.9% |

**Key finding:** the copula-simulated tail losses are notably larger than historical VaR alone suggests, implying stronger joint tail dependence between the two markets than a simple historical approach would capture — i.e. diversification benefits between the S&P 500 and Nikkei 225 weaken in stress periods.

## Repository Contents

- `individual_section.Rmd` — My section of the report: ARMA order selection, GARCH/TGARCH/eGARCH model comparison, conditional distribution selection, and residual diagnostics (see Credits below).
- `group_report.Rmd` — The full group report, included as supporting material for context (data collection, copula fitting, simulation, and final VaR/CVaR calculation).
- `README.md` — This file.

## Setup / Requirements

R packages: `quantmod`, `fitdistrplus`, `fGarch`, `rugarch`, `VineCopula`, `KScorrect`, `ADGofTest`, `FinTS`, `tseries`, `knitr`, `plotly`.

Market data is pulled live from Yahoo Finance via `quantmod::getSymbols()` at knit time — no local data files are required, but an internet connection is.

## Credits

This project was completed as coursework for UCL STAT0011 (Decision and Risk), as a group submission (Group 33). It is shared here as an individual portfolio piece, not a claim of sole authorship over the full pipeline.

My contribution (`individual_section.Rmd`) covered the marginal time series modelling stage:

- AIC-based ARMA order selection for both indices
- Comparing GARCH, TGARCH, and eGARCH volatility specifications
- Selecting the conditional distribution (evaluating 6 candidates, confirming skewed Student-t)
- Residual diagnostics: density/Q-Q plots, ACF, Ljung-Box and Engle's ARCH tests

The Probability Integral Transform, copula fitting, Monte Carlo simulation, and final VaR/CVaR calculation (visible in `group_report.Rmd`) were completed by my teammates.
