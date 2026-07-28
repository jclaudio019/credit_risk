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
| `00_data_preparation.ipynb` | Loads the source file and checks required fields. |
| `01_data_cleaning_and_exploration.ipynb` | Defines the historical target, repairs selected fields, and explores the portfolio. |
| `02_feature_engineering_and_woe.ipynb` | Creates coarse classes and uses WoE/IV as descriptive screening. |
| `03_pd_logistic_regression.ipynb` | Compares broader and refined logistic specifications for `P(good)`. |
| `04_pd_validation_and_scorecard.ipynb` | Converts `P(good)` to PD, reports AUC/Gini and a confusion matrix, and creates an illustrative 300–850 score. |

The refined specification uses grade, term, verification status, interest rate,
and debt-to-income ratio. Categorical values are one-hot encoded, with
`grade_G` used as the reference category. The notebooks use a stratified
80/20 train/test split; preprocessing statistics are learned from training rows
and then applied to held-out rows.

## Results

The final notebook is the authority for the executed AUC, Gini, confusion
matrix, and score distribution. It evaluates discrimination and threshold-based
classification on a held-out sample, then maps the model output to an
illustrative score range. No numerical performance claim is repeated here until
the notebooks have been executed with the local source data.

## Reproducibility

1. Obtain the historical Lending Club CSV and place it at
   `data/loan_data_2007_2014.csv`.
2. Create an environment and install `pip install -r requirements.txt`.
3. Run the five notebooks above in numerical order from the `notebooks/`
   directory so their relative data path resolves.

The source dataset is local and is not included in this repository. See
[`data/README.md`](data/README.md) for the expected location.

## Skills demonstrated

- Data-quality checks and transparent missing-value handling
- Exploratory analysis and historical target construction
- Coarse classing, Weight of Evidence, and Information Value
- Logistic regression, probability interpretation, AUC/Gini, and confusion matrices
- Clear limits on what a portfolio model can support

## Limitations

- `good_bad` is a simplified historical label, not a current underwriting rule.
- The random holdout is useful for this exercise but does not establish
  time-based stability, calibration, fairness, or regulatory suitability.
- The 300–850 scale is illustrative, not a production scorecard calibration or
  lending policy.
- Historical Lending Club data may not represent a current portfolio or lending
  environment.

## Future work

Before any real-world use, assess temporal validation, calibration, drift,
fairness, monitoring, and governance with appropriate data and review. A
separate project could explore operational delivery or loss modelling, but
those capabilities are outside this notebook portfolio.
