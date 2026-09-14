# AI-Powered Quantitative Trading for Netflix and Competitors

Exploratory quantitative research on Netflix and streaming-sector peers, combining macroeconomic and factor data with engineered price features to test whether tradable signals can be extracted.

This repository is a fork of my own earlier account (`asavari381`) into my primary account (`Asavari24`). Same author, not a fork of someone else's work.

Originally built as a graduate data science final project.

---

## Contents

| File | What it is |
|---|---|
| `DataScienceFinalPROJECT.ipynb` | The full analysis: data assembly, feature engineering, model comparison, and backtests |
| `Updated_feature_database_cleaned.csv` | The assembled feature database used by the notebook |
| `LICENSE` | License |

This is a notebook-based research project. There is no deployed service, container, or orchestration layer in this repository.

---

## Data

- **FRED** macroeconomic series (Federal Reserve Economic Data)
- **Fama-French** factor returns
- Engineered price and volume features derived from Netflix and streaming-sector peer price history

The feature database joins these sources onto a common daily index and is checkpointed to CSV so the modeling steps are reproducible without re-pulling external data.

---

## Methods

**Feature selection.** LASSO regression to shrink the engineered feature set and identify which inputs carry weight.

**Models compared.** Random Forest, XGBoost, and ARIMA specifications, evaluated against each other on the same feature database.

**Backtests.** Buy-and-hold and long-short strategies constructed from model output.

---

## Known limitations

These are stated plainly because they materially affect how the results should be read.

1. **The prediction target is the price level, not returns.** Price series are non-stationary and highly autocorrelated, so a model predicting tomorrow's price from recent prices largely learns that tomorrow's price is close to today's. This inflates R-squared and deflates RMSE without demonstrating predictive skill. The reported error improvements should therefore not be read as evidence of a tradable signal.

2. **No naive baseline comparison.** The models were not benchmarked against a random-walk or persistence baseline, which is the correct null for this problem.

3. **No transaction cost or slippage model.** Backtested strategy returns are gross. Any realistic cost assumption would reduce them, and for higher-turnover strategies could eliminate them entirely.

4. **Validation scheme is not purged.** Forward-looking overlap between training and evaluation windows has not been explicitly embargoed.

5. **No factor-adjusted alpha test.** Fama-French factors are used as model inputs but the strategy returns were never regressed against those factors, so it is untested whether any apparent outperformance is residual alpha or just repackaged factor exposure.

6. **Reported metrics in the original version of this README were unsubstantiated** and have been removed rather than restated.

---

## Roadmap

The changes that would make this defensible as signal research, in priority order:

1. **Switch the target to forward returns.** Predict log returns over a defined horizon rather than price level. Expect R-squared to collapse toward zero; that is the correct outcome, not a failure. Evaluate with information coefficient, hit rate, and strategy Sharpe rather than RMSE.
2. **Walk-forward validation with an embargo.** Expanding-window or rolling training with a purge gap between train and test to prevent leakage through overlapping forward returns.
3. **Make the strategy cross-sectional and dollar-neutral.** Rank the peer set each period by predicted return, go long the top and short the bottom in equal dollar amounts. This isolates relative performance from market direction.
4. **Add a transaction cost model.** Report Sharpe gross and net of a stated per-side cost assumption, plus turnover.
5. **Regress strategy returns on Fama-French factors.** Report the intercept and its significance. This is the test of whether the signal adds anything beyond known factors.

---

## Reproducing

Open `DataScienceFinalPROJECT.ipynb`. The feature database is included, so the modeling and backtest cells run without external API access.
