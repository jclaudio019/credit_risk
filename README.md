# Credit Risk Probability-of-Default Analysis

An educational, notebook-led analysis of historical Lending Club loans. The
project asks whether a small set of borrower and loan characteristics can rank
historical credit outcomes and be translated into an illustrative probability
of default (PD) view.

## Scope and business context

Credit-risk teams need a consistent way to summarize the relationship between
loan characteristics and observed repayment outcomes. This project develops a
transparent logistic-regression workflow for that question. It is a portfolio
analysis, not a lending decision system or production credit model.

The historical target is `good_bad`: `0` for the listed bad loan statuses
(`Charged Off`, `Default`, policy charged-off, and late 31–120 day loans), and
`1` for all other observed statuses. The model estimates probability of good standing, `P(good)`. PD is calculated only afterwards as `1 - P(good)`; the two should not be treated as the same quantity.

## Workflow

Run the notebooks in order.

| Notebook | Purpose |
| --- | --- |
| `00_data_understanding_and_preparation.ipynb` | Establishes the source fields, data quality context, and deterministic date conversions. |
| `01_data_cleaning_and_target_definition.ipynb` | Defines the historical target, applies partition-aware repairs, and creates the train/test split. |
| `02_discrete_feature_engineering_and_woe.ipynb` | Investigates discrete variables with train-derived WoE/IV and category groupings. |
| `03_continuous_feature_engineering_and_woe.ipynb` | Performs fine/coarse classing for ordered and continuous predictors. |
| `04_pd_logistic_regression_and_validation.ipynb` | Fits logistic specifications for `P(good)` and evaluates held-out discrimination and thresholds. |
| `05_pd_scorecard_and_final_conclusions.ipynb` | Builds the illustrative 300–850 scorecard and interprets held-out account scores. |

## Methodology

The workflow begins with data understanding, deterministic cleaning, and the
historical target definition. It then uses a stratified 80/20 train/test split,
with imputation statistics, category definitions, and feature bins learned
from training rows and applied unchanged to held-out rows.

Discrete variables are reviewed with Weight of Evidence (WoE) and Information
Value (IV); ordered and continuous variables are examined through fine and
coarse classing. The selected feature families use explicit reference
categories in logistic regression. The notebooks report coefficients,
probability interpretation, held-out threshold behavior, ROC/AUC, Gini, KS,
and an illustrative 300–850 scorecard. The model estimates `P(good)` and
calculates PD explicitly as `1 - P(good)`.

The illustrative score scale is derived from the theoretical per-family
minimum and maximum model coefficients, including the intercept, rather than
from the observed training-score distribution.

## Results

The freshly executed held-out model reports AUC **0.699482203848** and Gini
**0.398964407696**. At the displayed 0.5 `P(good)` threshold, the confusion
matrix for actual labels `[0, 1]` is `[[10, 10184], [15, 83048]]`; bad-class
recall is **10 / 10194 = 0.000980969198**, or approximately **0.10%**. These
figures show useful but limited rank ordering and an unsuitable default
classification threshold, not a lending decision rule. The final notebook
maps the model output to the illustrative score range described above.

## Reproducibility

1. Obtain the historical Lending Club CSV and place it at
   `data/loan_data_2007_2014.csv`.
2. Create an environment and install `pip install -r requirements.txt`.
3. Run the six notebooks above in numerical order from the `notebooks/`
   directory so their relative data path resolves.

The source dataset is local and is not included in this repository. See
[`data/README.md`](data/README.md) for the expected location.

## Limitations

- `good_bad` is a simplified historical bad-status proxy, not a fixed
  performance-horizon default definition or current underwriting rule.
- The random holdout is useful for this exercise but does not establish
  time-based stability, calibration, fairness, or regulatory suitability.
- The 300–850 scale is illustrative, not a production scorecard calibration or
  lending policy.
- Historical Lending Club data may not represent a current portfolio or lending
  environment.
