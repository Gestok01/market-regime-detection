# Market Regime Detection using Hidden Markov Models

## Overview

Financial markets do not behave uniformly over time. Periods of sustained growth, sharp drawdowns, high volatility, and sideways consolidation represent **different market regimes**, and most trading or risk strategies fail because they ignore these regime shifts.

This project builds a **market regime detection system** using **unsupervised machine learning**, with a focus on **time-aware modeling** via **Hidden Markov Models (HMMs)**. The system infers latent market states from historical data and validates them using economic and statistical reasoning rather than predictive accuracy alone.

The project is designed to reflect **real-world macro and risk modeling workflows**, not toy ML examples.

---

## Problem Statement

> **Can we automatically infer hidden market regimes (bull, bear, volatile, sideways) from historical market data without labeled examples?**

Key challenges:
- Market regimes are **latent** (not directly observable)
- Financial time series are **non-stationary**
- Regimes exhibit **persistence and transitions**, which standard clustering ignores

---

## Why Market Regimes Matter

Market behavior changes drastically across regimes:
- Strategies that work in bull markets fail in crises
- Risk characteristics vary significantly across volatility regimes
- Asset allocation decisions depend on regime context

As a result, **macro funds, asset allocators, and risk desks** often condition decisions on regime estimates rather than raw signals.

---

## Design Decisions (Key Ideas)

### 1. Weekly Data Instead of Daily
- Regimes are **macro phenomena**, not daily noise
- Weekly aggregation suppresses microstructure noise
- Results in more stable and interpretable regimes

Daily data is used **only for controlled comparison**, not as the primary signal.

---

### 2. Behavioral Features (Not Prices)

Raw prices are never used directly. Instead, the model relies on **behavioral features**:

- Log returns (directional behavior)
- Rolling volatility (uncertainty)
- Trend strength
- VIX level (market fear)
- VIX changes (shock sensitivity)

This avoids scale dependence and improves statistical robustness.

---

### 3. Why Clustering Is Not Enough

K-Means and Gaussian Mixture Models are used as baselines to demonstrate limitations:
- No temporal awareness
- Frequent regime switching
- Poor persistence modeling

This motivates the use of **Hidden Markov Models**.

---

### 4. Hidden Markov Models (Core Model)

HMMs are well-suited for regime detection because they:
- Model **latent states**
- Explicitly capture **state persistence**
- Learn **transition probabilities** between regimes
- Produce probabilistic state assignments

A Gaussian HMM is used due to continuous feature inputs.

---

## Model Selection

The number of regimes is selected using **information criteria**:
- Akaike Information Criterion (AIC)
- Bayesian Information Criterion (BIC)

This avoids arbitrary state selection and demonstrates bias–variance tradeoffs.  
Weekly data exhibits a clear BIC minimum, while daily data tends to overfit.

---

## Regime Interpretation & Validation

After training, inferred HMM states are interpreted **post hoc** using:

- Average returns
- Volatility
- Trend behavior
- VIX levels
- Price distribution characteristics

Regimes are mapped to economically meaningful labels:
- **Bull / Expansion**
- **Bear / Crisis**
- **Sideways / Low Volatility**
- **Volatile / Transition**

Validation is performed using **returns and volatility**, not absolute price levels, to avoid non-stationarity bias.

---

## Project Structure

market-regime-detection/
│
├── data/
│   └── raw/
│       ├── sp500_daily.csv
│       ├── sp500_weekly.csv
│       ├── features_daily.csv
│       ├── features_weekly.csv
│       └── weekly_regimes_labeled.csv
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_clustering_comparison.ipynb
│   ├── 04_hmm_regime_comparison.ipynb
│   └── 05_hmm_model_selection.ipynb
│
├── requirements.txt
└── README.md

---

## Typical Usage (Production Perspective)

This system is intended for **batch regime inference**, not real-time trading.

A typical workflow:
1. Update weekly market data
2. Run regime inference
3. Use inferred regime for:
   - Risk monitoring
   - Strategy conditioning
   - Asset allocation decisions

The design mirrors how regime models are used in practice.

---

## What This Project Does NOT Claim

- ❌ It does not predict prices
- ❌ It does not optimize trading performance
- ❌ It does not provide real-time signals

The goal is **regime understanding and structural market analysis**, not alpha generation.

---

## Key Takeaways

- Market regimes are latent, persistent, and probabilistic
- Time-aware models outperform static clustering
- Weekly aggregation improves regime stability
- HMMs provide interpretable transition dynamics
- Proper validation avoids non-stationarity pitfalls

---

## Future Extensions (Optional)

- Multi-asset regime analysis
- Regime transition stress testing
- Macro indicator integration
- Regime-conditioned strategy evaluation

---

## Author

Built by **Uday Tyagi** as a portfolio-grade ML + quantitative finance project.


