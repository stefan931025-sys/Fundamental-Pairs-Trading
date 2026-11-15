# Fundamental Pairs Trading

![CI Workflow](https://github.com/stefan931025-sys/Fundamental-Pairs-Trading/actions/workflows/ci.yml/badge.svg)
![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

Quantitative pairs trading engine combining fundamental co-driver validation (e.g., payment networks like Visa and Mastercard) with rigorous statistical cointegration, Engle-Granger two-step testing, and dynamic Z-score mean-reversion execution.

## 🚀 Strategy & Architecture

* **Fundamental Drivers:** Pairs are selected not merely on statistical correlation, but on shared macroeconomic exposure, pricing power parity, and structural operating leverage.
* **Relative Value Dislocation:** While long-term business economics are cointegrated, short-term price spreads diverge due to transitory institutional flow imbalances, localized earnings reporting variances, or asymmetric liquidity shocks.

---

## 📊 Quantitative Architecture & Signal Mechanics

### 1. Engle-Granger Two-Step Cointegration Test
To prevent entering "spurious regressions" (assets that look correlated but have zero economic linkage), the engine continuously verifies the stationarity of the spread:
* **OLS Regression Analysis:** Runs an Ordinary Least Squares (OLS) regression of asset prices to derive the hedge ratio ($\hat{\beta}$).
* **Augmented Dickey-Fuller (ADF) Test:** Evaluates the residuals of the cointegrating regression to confirm mean-reverting properties.

### 2. Z-Score Execution Banding
* **Dynamic Spread Calculation:** Calculates rolling Z-scores of the cointegration residuals.
* **Execution Triggers:** Automatically scales entries when the spread exceeds historical volatility thresholds ($\pm 2.0\sigma$) and exits upon mean reversion to baseline parity.

---

## 📐 Mathematical Specifications

### 1. Cointegrating Regression & Spread

$$Y_{1, t} = \alpha + \beta Y_{2, t} + \varepsilon_t, \quad S_t = Y_{1, t} - \hat{\beta} Y_{2, t}$$

Where $\hat{\beta}$ is the OLS-estimated hedge ratio and $S_t$ represents the stationary spread residual.

### 2. Augmented Dickey-Fuller (ADF) Unit Root Test

$$\Delta S_t = \gamma S_{t-1} + \sum_{i=1}^{k} \delta_i \Delta S_{t-i} + u_t$$

Testing the null hypothesis $H_0: \gamma = 0$ (non-stationary unit root) against $H_1: \gamma < 0$ (stationary cointegrated spread).

### 3. Normalized Z-Score Entry Signal

$$Z_t = \frac{S_t - \mu_s}{\sigma_s}$$

---

## 🔄 System Architecture & Data Flow

```text
       VISA (V) & MASTERCARD (MA) PRICE FEEDS
     (Synchronized Daily Close & Intraday Tick Arrays)
                           │
                           ▼
       ENGLE-GRANGER COINTEGRATION & SPREAD ENGINE
  * OLS Hedge Ratio ($\hat{\beta}$) Estimation  * ADF Stationarity
  * Rolling Mean ($\mu_s$) & StDev ($\sigma_s$) * Dynamic Z-Score
                           │
                           ▼
               MARKET-NEUTRAL EXECUTION TERMINAL
     (Spread Divergence Triggers, Position Sizing, & Risk Controls)
