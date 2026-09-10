# Bayesian Volatility Modeling for Stock Returns

A quantitative finance project analyzing and estimating the volatility dynamics of stock returns using **GARCH** and **Bayesian Stochastic Volatility (SV) models**.

The project focuses on:

- Maximum-likelihood estimation of GARCH(1,1) models
- Bayesian estimation of stochastic volatility models
- Markov Chain Monte Carlo (MCMC)
- MCMC convergence diagnostics
- Comparison of volatility dynamics between GOOG and MSFT

## Overview

Financial returns often exhibit **volatility clustering**, where periods of large price fluctuations tend to be followed by further periods of high volatility.

This project investigates this phenomenon from two perspectives:

1. **GARCH(1,1)** modeling to examine volatility persistence.
2. **Bayesian Stochastic Volatility modeling** to estimate latent volatility in real stock-market data.

For the empirical analysis, I use historical daily data for:

- Alphabet Inc. (GOOG)
- Microsoft Corporation (MSFT)

The dataset covers **2021 to May 20, 2025**, with **1,101 observations for each stock**.

---

## 1. GARCH(1,1) Model

The return process is modeled as

$$
R_t = a + bR_{t-1} + \epsilon_t
$$

with

$$
\epsilon_t = \sigma_t z_t
$$

and conditional variance

$$
\sigma_t^2
=
\omega
+
\beta \sigma_{t-1}^2
+
\alpha \epsilon_{t-1}^2
$$

The persistence of volatility shocks is determined by

$$
\alpha+\beta.
$$

A value close to 1 indicates that volatility shocks decay slowly.

### Estimated Persistence

| Series | α + β |
|---|---:|
| XP1 | 0.9869 |
| XP3 | 0.9439 |
| XP2 | 0.8834 |

XP1 therefore exhibits the strongest volatility persistence, followed by XP3 and XP2.

---

## 2. Bayesian Stochastic Volatility Model

To model time-varying latent volatility, I implemented a stochastic volatility model.

### Observation Model

$$
y_t = e^{h_t/2}z_t,
\qquad
z_t \sim N(0,1)
$$

### State Model

The latent log-volatility follows an AR(1) process:

$$
h_{t+1}
=
\mu
+
\phi(h_t-\mu)
+
\eta_t,
$$

where

$$
\eta_t \sim N(0,\sigma^2).
$$

The initial state is

$$
h_1
\sim
N\left(
\mu,
\frac{\sigma^2}{1-\phi^2}
\right).
$$

### Priors

The Bayesian model uses the following prior distributions:

$$
\frac{\phi+1}{2}
\sim
\mathrm{Beta}(20,1.5)
$$

$$
\mu \sim N(1,1)
$$

$$
\sigma^2 \sim IG(2,0.2).
$$

The joint posterior distribution is therefore proportional to

$$
p(\mu,\phi,\sigma,h_{1:T}\mid y_{1:T})
\propto
p(y_{1:T}\mid h_{1:T})
p(h_{1:T}\mid\mu,\phi,\sigma)
p(\mu)p(\phi)p(\sigma).
$$

---

## 3. Data

Daily stock-price data were collected from NASDAQ.

| Stock | Mean Price | Price SD | Mean Return | Return SD |
|---|---:|---:|---:|---:|
| GOOG | 135.14 | 27.95 | -0.0590 | 1.9798 |
| MSFT | 327.33 | 70.16 | -0.0677 | 1.6808 |

The analysis uses approximately log-transformed daily returns.

---

## 4. MCMC Estimation

One of the main focuses of this project was examining how the number of MCMC iterations affects posterior estimation.

I compared:

- 100 iterations
- 1,000 iterations
- 5,000 iterations

With only 100–1,000 samples, the posterior distribution of the volatility parameter $\sigma$ showed apparent multimodality and unstable traces.

Increasing the number of iterations to **5,000** substantially improved posterior stability and produced smoother, approximately unimodal posterior distributions.

This experiment highlights the importance of **convergence diagnostics and sufficient posterior exploration in MCMC estimation**.

---

## 5. Results

### GOOG

Using 5,000 MCMC iterations:

| Parameter | Posterior Mean | SD | 95% Credible Interval |
|---|---:|---:|---|
| φ | 0.8617 | 0.0440 | [0.7584, 0.9276] |
| μ | 1.0574 | 0.1052 | [0.8524, 1.2661] |
| σ | 0.3975 | 0.0653 | [0.2898, 0.5578] |

### MSFT

Using 5,000 MCMC iterations:

| Parameter | Posterior Mean | SD | 95% Credible Interval |
|---|---:|---:|---|
| φ | 0.9077 | 0.0346 | [0.8213, 0.9572] |
| μ | 0.7862 | 0.1178 | [0.5553, 1.0259] |
| σ | 0.2992 | 0.0605 | [0.2098, 0.4469] |

Both stocks exhibit strong volatility persistence, with $\phi$ close to 0.9.

---

## 6. GOOG vs. MSFT

The estimated latent volatility shows similar broad temporal patterns for the two stocks, suggesting exposure to common market-wide shocks.

However, several differences emerge:

- GOOG generally exhibits higher estimated volatility.
- GOOG also shows larger volatility spikes.
- MSFT's volatility is relatively more concentrated at lower levels.
- Both stocks exhibit clear volatility clustering.
- The high estimated $\phi$ values indicate persistent volatility dynamics.

Overall, **GOOG appears somewhat more volatile and unstable than MSFT during the analyzed period**.

---

## Key Takeaways

This project demonstrates how volatility can be analyzed using both classical and Bayesian time-series methods.

In particular, I gained practical experience with:

- Financial time-series preprocessing
- GARCH modeling
- Bayesian state-space models
- Latent-variable estimation
- MCMC sampling
- Posterior inference
- Convergence diagnostics
- Statistical comparison of financial assets

A particularly important observation was that insufficient MCMC iterations can produce misleading posterior shapes. Increasing the number of samples substantially improved the stability of the posterior estimates.

---

## Tech Stack

- R
- Time-Series Analysis
- Bayesian Statistics
- MCMC
- GARCH
- Stochastic Volatility Models
- Data Visualization

## Data

Historical daily stock-price data for **GOOG** and **MSFT** were obtained from NASDAQ.

Period: **2021 – May 20, 2025**

Number of observations: **1,101 per stock**

---

## Author

**Xin Tan**

Economics / Quantitative Finance / Machine Learning
