# Final Report — Credit Risk Probability-of-Default Analysis

## Executive summary

This project is an educational analysis of historical Lending Club loans. It
uses logistic regression to estimate probability of good standing from a small,
interpretable set of loan characteristics, then derives probability of default
as `PD = 1 - P(good)`. The work demonstrates a complete notebook workflow from
source checks through feature review, model fitting, held-out evaluation, and
an illustrative scorecard layer.

The notebook outputs are the metric authority. They should be executed with the
local source CSV before quoting AUC, Gini, confusion-matrix counts, or score
distribution values.

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

Notebook 01 makes selected data repairs explicit: employment length and term
are converted to numeric values, dates are parsed, income is filled with the
sample median, and missing revolving-credit limits use funded amount. Notebook
02 creates dummy variables and coarse bands, then uses Weight of Evidence and
Information Value as descriptive feature-screening tools.

The logistic-regression workflow compares a broader candidate specification
with a refined one. The refined raw inputs are grade, term, verification
status, interest rate, and debt-to-income ratio. The data is split into
stratified training and held-out sets. Numeric imputation values are calculated
from the training rows only, categorical variables are one-hot encoded, and
`grade_G` is dropped as the reference category.

## Interpretation and validation

The fitted model estimates `P(good)`, not PD directly. The final notebook
calculates `PD = 1 - P(good)` for held-out rows. It reports AUC and Gini for
rank ordering, plus a confusion matrix at a 0.5 probability-of-good-standing
threshold. Threshold-based counts depend on that chosen threshold and should
not be read as a lending policy recommendation.

At the displayed 0.5 `P(good)` threshold, no held-out bad rows are predicted
bad, so bad-class recall is zero. Threshold selection and calibration are
required before classification use; AUC and Gini still describe ranking
discrimination.

The final notebook also linearly rescales the held-out model logit to an
illustrative 300–850 score. The scale is anchored using the training score
distribution and then applied unchanged to held-out rows. It aids explanation;
it is not calibrated as a production scorecard.

## Notebook map

| Notebook | Contribution |
| --- | --- |
| `00_data_preparation.ipynb` | Checks the local source file and required columns. |
| `01_data_cleaning_and_exploration.ipynb` | Defines `good_bad`, performs selected cleaning, and explores the portfolio. |
| `02_feature_engineering_and_woe.ipynb` | Builds coarse classes and reviews WoE/IV. |
| `03_pd_logistic_regression.ipynb` | Fits broader and refined logistic models for `P(good)`. |
| `04_pd_validation_and_scorecard.ipynb` | Derives PD, evaluates held-out discrimination, and creates the illustrative score view. |

## Limitations and next phases

- The source data and target are historical and simplified.
- A single random holdout does not demonstrate temporal stability, calibration,
  fairness, governance, or regulatory suitability.
- The score range is illustrative and does not define approval, pricing, or
  treatment rules.
- Any practical use would require separate validation, monitoring, governance,
  and domain review.

Reasonable future analysis would begin with time-based validation and
calibration assessment, followed by drift and fairness studies. Operational
delivery or loss modelling are separate future scopes, not implemented here.
