# Relative Value Equity Pairs Trading Framework

[![Relative Value Equity Pairs Trading CI](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/tshepokotelo95-commits/Fundamental-Pairs-Trading)
[![Python Version](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A systematic statistical arbitrage environment built to exploit structural pricing dislocations within closely coupled corporate duopolies. This framework implements an Engle-Granger two-step cointegration pipeline to bypass superficial price correlations, isolating pure fundamental relative value anomalies.

---

## Investment Thesis: The Visa (V) vs. Mastercard (MA) Duopoly

Trading individual equity names exposes capital to severe macro, interest rate, and idiosyncratic sector risks. This framework targets a structural duopoly—Visa (V) and Mastercard (MA)—to mitigate broad market beta and harvest clean alpha.

### 1. Economic Moat & Structural Alignment
* **Network Parallelism:** Both corporations operate near-identical capital-light payment processing tollbooths, scaling synchronously with global consumption and inflation layers.
* **Operating Leverage:** Because their cost structures are highly fixed, variations in their respective gross processing volumes (GPV) directly flow to operating margins under identical macroeconomic conditions.

### 2. Relative Value Dislocation
While their long-term underlying business economics are cointegrated, short-term price spreads diverge due to transitory institutional flow imbalances, localized earnings reporting variances, or asymmetric liquidity shocks. This framework programmatically quantifies these temporary pricing gaps to execute market-neutral mean-reversion trades.

---

## Quantitative Architecture & Signal Mechanics

The model abandons naïve chart-pattern tracking in favor of a mathematically rigorous relative value framework:

### 1. Engle-Granger Two-Step Cointegration Test
To prevent entering "spurious regressions" (assets that look correlated but have zero economic linkage), the engine continuously verifies the stationarity of the spread:
* **OLS Regression Analysis:** Runs an Ordinary Least Squares (OLS) regression of asset prices to derive the hedge ratio ($\beta$).
* **Augmented Dickey-Fuller (ADF) Test:** Evaluates the residuals of the cointegrating regression to confirm mean-reverting properties.

### 2. Z-Score Execution Banding
* **Dynamic Spread Calculation:** Calculates rolling Z-scores of the cointegration residuals.
* **Execution Triggers:** Automatically scales entries when the spread exceeds historical volatility thresholds ($\pm 2.0\sigma$) and exits upon mean reversion to baseline parity.

---

## Mathematical Specifications

### 1. Cointegrating Regression & Spread
$$Y_{1,t} = \alpha + \beta Y_{2,t} + \varepsilon_t$$
$$S_t = Y_{1,t} - \hat{\beta} Y_{2,t}$$
Where $\hat{\beta}$ is the OLS-estimated hedge ratio and $S_t$ represents the stationary spread residual.

### 2. Augmented Dickey-Fuller (ADF) Unit Root Test
$$\Delta S_t = \gamma S_{t-1} + \sum_{i=1}^{k} \delta_i \Delta S_{t-i} + u_t$$
Testing the null hypothesis $H_0: \gamma = 0$ (non-stationary unit root) against $H_1: \gamma < 0$ (stationary cointegrated spread).

### 3. Normalized Z-Score Entry Signal
$$Z_t = \frac{S_t - \mu_S}{\sigma_S}$$

---

## System Architecture & Data Flow

```text
+-----------------------------------------------------------------------------------+
|                       VISA (V) & MASTERCARD (MA) PRICE FEEDS                      |
|                (Synchronized Daily Close & Intraday Tick Arrays)                  |
+-----------------------------------------------------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|                     ENGLE-GRANGER COINTEGRATION & SPREAD ENGINE                   |
|  * OLS Hedge Ratio ($\hat{\beta}$) Estimation   * ADF Stationarity Residual Test  |
|  * Rolling Mean ($\mu_S$) & StDev ($\sigma_S$)  * Dynamic Z-Score Normalization   |
+-----------------------------------------------------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|                        MARKET-NEUTRAL EXECUTION TERMINAL                          |
|           (Spread Divergence Triggers, Position Sizing, & Risk Controls)          |
+-----------------------------------------------------------------------------------+
