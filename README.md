# Synthetic Forecasting Case Study: Negative R2 Under a Regime Shift

[![scikit-learn](https://img.shields.io/badge/scikit--learn-R2%20%7C%20LinearRegression-orange)](https://scikit-learn.org/stable/)
[![statsmodels](https://img.shields.io/badge/statsmodels-ADF%20%7C%20KPSS%20%7C%20Zivot--Andrews%20%7C%20MarkovRegression-blue)](https://www.statsmodels.org/)
[![Chronos](https://img.shields.io/badge/Amazon%20Chronos-T5--mini-green)](https://www.amazon.science/publications/chronos-learning-the-language-of-time-series)

This repository contains the synthetic notebook that supports the article:

> Article link: TODO

The notebook does not contain real business data. It recreates a failure pattern with synthetic monthly data in order to illustrate a diagnostic workflow for a forecasting problem where several models return negative R2 after a severe regime shift.

## Motivation

The case behind the article started with a forecasting model that kept failing with negative R2 values. A natural first reaction in that situation is to keep trying new models, new features, and new hyperparameters.

This repository explores a different path: before continuing with model hopping, inspect the assumptions behind the data-generating process. The goal is not to prove that one model family is better than another, but to show why diagnostic reasoning and production judgment matter before iterating without a clear hypothesis.

## Methodology

The notebook builds a synthetic monthly time series with two different regimes:

- a stable regime covering roughly the first 70% of the series;
- a crisis-like regime covering the remaining 30%;
- an extreme spike around the transition point;
- a persistent higher-volatility period after the transition.

The diagnostic workflow includes:

- ADF and KPSS stationarity tests;
- Zivot-Andrews testing for a structural break;
- Markov Regression with two regimes and switching variance;
- a forecasting benchmark using four alternatives:
  - naive last-value baseline;
  - linear regression;
  - XGBoost regressor;
  - Amazon Chronos-T5-mini for zero-shot forecasting.

## Results

In the synthetic setup, the evaluation window belongs to the crisis-like regime while most of the available historical context comes from the previous stable regime.

Under that configuration, all evaluated forecasting alternatives return negative R2 values. The result is intended to reproduce the failure pattern discussed in the article: when the underlying process changes sharply, switching model families may not be enough to produce a forecast that is trustworthy for production.

## How to Run

This project uses `uv` for dependency management.

```bash
uv sync
```

Then open the notebook:

```bash
uv run jupyter notebook comparision_xgboost_naive_chronos.ipynb
```

The notebook was originally executed in Google Colab using the free Tesla GPU runtime. Chronos can be the heaviest part of the workflow, so using a GPU runtime is recommended if you want to reproduce the full benchmark comfortably.

## Limitations

This repository is intentionally narrow in scope.

- The data is synthetic and does not represent any real company, client, or proprietary dataset.
- The benchmark is not a robust or exhaustive comparison of forecasting algorithms.
- The results should not be interpreted as evidence that XGBoost, Chronos, linear regression, or naive baselines are generally better or worse forecasting methods.
- Hyperparameter tuning is not the focus of the notebook.
- The purpose is to recreate a real-world failure pattern and the reasoning around it, not to build a production-grade forecasting benchmark.

## Privacy and Confidentiality

No proprietary data is included in this repository.

The original business context has been anonymized, and the notebook uses synthetic data only. The figures and results are conceptual reproductions designed to support the article's technical discussion.

## References

- scikit-learn: https://scikit-learn.org/stable/
- statsmodels: https://www.statsmodels.org/
- Amazon Chronos: https://www.amazon.science/publications/chronos-learning-the-language-of-time-series
