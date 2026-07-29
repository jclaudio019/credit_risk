# Final Report — Credit Risk Probability-of-Default Analysis

## Executive summary

This project is an educational analysis of historical Lending Club loans. It
uses logistic regression to estimate probability of good standing from a small,
interpretable set of loan characteristics, then derives probability of default
as `PD = 1 - P(good)`. The work demonstrates a complete notebook workflow from
source checks through feature review, model fitting, held-out evaluation, and
an illustrative scorecard layer.

The freshly executed held-out model has AUC **0.699482203848** and Gini
**0.398964407696**. At the displayed 0.5 `P(good)` threshold, the confusion
matrix for actual labels `[0, 1]` is `[[10, 10184], [15, 83048]]`; bad-class
recall is **10 / 10194 = 0.000980969198**, or approximately **0.10%**. This is
useful but limited ranking evidence and an unsuitable classification threshold,
not a lending decision recommendation.

## Dataset and target

The notebooks expect the historical Lending Club file at
`data/loan_data_2007_2014.csv`. Required fields include loan status,
employment length, term, credit-line and issue dates, income, funded amount,
revolving-credit limit, grade, interest rate, and debt-to-income ratio.

The target is `good_bad`. The following historical statuses are labelled bad
(`0`): Charged Off, Default, policy charged-off, and late 31–120 day loans. All
other observed statuses are labelled good standing (`1`). This is a simplified
educational target; it is not a current lending decision or policy definition.

## Method

The six-notebook workflow begins with data understanding and preparation,
then defines the historical target and applies cleaning decisions before a
stratified 80/20 train/test split. Employment length and term are converted to
numeric values; dates are parsed with a historical-date correction; missing
revolving-credit limits use funded amount; and annual-income imputation is
learned from training rows only.

The feature-engineering notebooks examine discrete variables with Weight of
Evidence (WoE) and Information Value (IV), then apply fine and coarse classing
to ordered and continuous variables. Category definitions, bins, and reference
levels are derived from training rows and carried unchanged to held-out rows.
The logistic-regression notebook fits broader and statistically interpretable
specifications for `P(good)`, reporting coefficients and p-values. The final
notebook converts the fitted model into an illustrative
300–850 scorecard with explicit zero-coefficient reference categories.

## Interpretation and validation

The fitted model estimates `P(good)`, not PD directly. The final notebook
calculates `PD = 1 - P(good)` for held-out rows. It reports AUC and Gini for
rank ordering, plus a confusion matrix at a 0.5 probability-of-good-standing
threshold. Threshold-based counts depend on that chosen threshold and should
not be read as a lending policy recommendation.

At the displayed 0.5 `P(good)` threshold, 10 of 10,194 held-out bad rows are
predicted bad, so bad-class recall is approximately 0.10%. Threshold selection
and calibration are required before classification use; AUC and Gini still
describe ranking discrimination.

The final notebook also linearly rescales the model logit to an illustrative
300–850 score. The endpoints come from the theoretical sum of the minimum and
maximum coefficient in each feature family, including the intercept, rather
than from the observed training-score distribution. The resulting mapping is
then applied unchanged to held-out rows. It aids explanation; it is not
calibrated as a production scorecard.

## Notebook map

| Notebook | Contribution |
| --- | --- |
| `00_data_understanding_and_preparation.ipynb` | Establishes source fields, data quality context, and date preparation. |
| `01_data_cleaning_and_target_definition.ipynb` | Defines `good_bad`, applies partition-aware cleaning, and creates the split. |
| `02_discrete_feature_engineering_and_woe.ipynb` | Investigates discrete predictors with WoE/IV and category grouping. |
| `03_continuous_feature_engineering_and_woe.ipynb` | Applies fine/coarse classing to ordered and continuous predictors. |
| `04_pd_logistic_regression_and_validation.ipynb` | Fits and validates held-out `P(good)` predictions. |
| `05_pd_scorecard_and_final_conclusions.ipynb` | Builds the illustrative scorecard and concludes the analysis. |

## Limitations

- The source data and target are historical and simplified; the bad-status
  proxy does not define a fixed performance horizon.
- A single random holdout does not demonstrate temporal stability, calibration,
  fairness, or regulatory suitability.
- The score range is illustrative and does not define approval, pricing, or
  treatment rules.
