# sf-acceleration

Research on momentum acceleration as a cross-sectional equity signal using `sf_quant` and Polars.

## Overview

This repo contains the notebook and supporting work for testing whether **momentum acceleration** has predictive power in equity returns.

The original idea was simple:

- measure whether momentum is **increasing or decreasing**
- test whether that change in momentum predicts next-period returns
- keep the signal construction simple and interpretable

The baseline acceleration signal is:

\[
\text{Acceleration} = R_{t-1:t-3} - R_{t-4:t-6}
\]

where:

- \(R_{t-1:t-3}\) is the compounded return over the most recent 3 months
- \(R_{t-4:t-6}\) is the compounded return over the prior 3 months

The project tests three main specifications:

1. **Raw return acceleration**
2. **Acceleration conditioned on momentum (double sort)**
3. **Specific return acceleration** using idiosyncratic returns

## Main Finding

Momentum acceleration does **not** behave like a standard linear factor.

A simple long-short portfolio based on highest minus lowest acceleration does not work well. However, the results suggest a **nonlinear relationship**:

- **moderate acceleration** tends to outperform
- **extreme acceleration** tends to underperform

This suggests acceleration may be more useful as a **filter** or **regime signal** than as a standalone directional alpha factor.

## Repository Contents

- `acceleration.ipynb`  
  Main research notebook containing:
  - data loading with `sf_quant`
  - daily-to-monthly aggregation
  - signal construction
  - decile analysis
  - double-sort analysis
  - specific return acceleration tests
  - cumulative return plots and summary tables

## Data / Environment

This project was built in a BYU Silver Fund research environment using:

- `sf_quant`
- `polars`
- `matplotlib`
- CRSP-style return data accessed through the shared `sf_quant` pipeline

Because the raw data is not public, this repo is primarily intended to document:
- the research process
- the signal definitions
- the testing framework
- the conclusions

## Method Summary

The workflow in the notebook is roughly:

1. Load daily equity data from `sf_quant`
2. Aggregate daily returns to monthly returns using log compounding
3. Construct lagged return windows
4. Build acceleration signals
5. Sort stocks into deciles
6. Evaluate:
   - mean decile returns
   - annualized Sharpe ratios
   - long-short spreads
   - cumulative log return plots

## Result Summary

### Raw Acceleration
The raw signal does not produce monotonic decile returns. Middle deciles outperform both extremes.

### Double Sorted Acceleration
Conditioning on momentum does not restore monotonicity. The same inverted-U shape remains.

### Specific Return Acceleration
Using specific returns does not materially change the result, suggesting the nonlinear structure is not primarily driven by market or factor exposure.

### Moderate vs Extreme Acceleration
A spread that goes long moderate-acceleration stocks and short extreme-acceleration stocks performs better than the standard top-minus-bottom acceleration spread, reinforcing the idea that acceleration is nonlinear.

## Interpretation

The evidence suggests that:

- very high acceleration may reflect **crowded, late-stage momentum**
- very low acceleration may reflect **breaking or unstable trends**
- moderate acceleration may reflect **more stable continuation**

In other words, the signal may be capturing **instability / overextension** rather than pure trend strength.

## Next Steps

Potential follow-up work includes:

- Information Coefficient (IC) analysis
- implementing the signal cleanly in the `sf_quant` research pipeline
- full backtesting with realistic portfolio constraints
- testing nonlinear transformations explicitly
- comparing mid-vs-extreme signals against standard momentum

## Author

**Chase Nielsen**  
BYU Silver Fund
