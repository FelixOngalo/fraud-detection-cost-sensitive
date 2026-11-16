# Fraud Detection: Rare Events, Calibration and Cost-Based Decisions

A supervised fraud-detection project in R on simulated card transactions. It
covers what most tutorials skip: rare-event metrics, the effect of class
weighting and downsampling on probabilities, calibration, decisions that
minimise cost (including an analyst-capacity limit), and what happens when
fraudsters change tactics.

## Question

Which transactions should be sent to an analyst, given that alerts cost money,
missed fraud costs more, fraud is about 1% of transactions, and the pattern
changes over time?

## Key findings

*(Fill in after running: fraud rate and the accuracy of a model that flags
nothing, best model and its PR-AUC, cost saved against no model on the test
period, and recall by fraud type before and after the new tactic.)*

## What is in it

- **Data preparation** with a log of every fix, a time-based train / validation /
  test split, and a customer-history feature built only from earlier transactions
- **Five models** (logistic and random forest; plain, class-weighted, downsampled)
  compared with ROC-AUC, PR-AUC, and precision and recall in the top 1%
- **Calibration:** reliability tables and Platt scaling, and why weighting and
  downsampling distort probabilities
- **Cost-based decisions:** flag when p x loss exceeds the review cost, compared
  with a fixed threshold, a top-1% rule, and a daily capacity limit, all chosen on
  validation and evaluated on test
- **Drift:** a new fraud tactic appears in the last four weeks; recall by fraud type,
  population stability index, and a test of whether retraining helps

## Tools

R, dplyr, tidyr, tibble, readr, ranger, ggplot2, Quarto. Metrics, calibration and
cost functions are written from scratch.

## Reproduce

1. `install.packages(c("dplyr", "tidyr", "tibble", "readr", "ranger", "ggplot2", "scales", "knitr", "quarto"))`
2. Run in order:
   - `R/00_simulate_data.R` writes the raw transactions and the hidden fraud types
   - `R/01_prepare_explore.R` cleaning, features, split, exploration
   - `R/02_models.R` five models, calibration, metrics
   - `R/03_decisions.R` cost-based policies, drift analysis, retraining test
3. Render `reports/report.qmd`

## Project structure

```
R/fraud_functions.R        simulator, features, metrics, calibration, cost rules, model fitting
R/00_simulate_data.R       raw data
R/01_prepare_explore.R     preparation and exploration
R/02_models.R              models and calibration
R/03_decisions.R           decisions, drift, retraining
reports/report.qmd         written analysis and charts
data/raw/                  simulated raw data (generated)
data/clean/                prepared data and saved results (generated)
```

## Limitations

- Simulated data that is easier to separate than real fraud, so real PR-AUC
  values would be lower
- Labels are treated as immediately known; in practice fraud is confirmed days
  or weeks later
- Small test set (about 180 frauds), so differences of a few percent between
  policies are within noise
- Alerts are assumed to stop fraud perfectly and to cost a fixed $5
- Fraud adapts to the model, so historical performance overstates future
  performance
