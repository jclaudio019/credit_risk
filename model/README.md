# Lightweight PD model artifacts

This directory contains an inspectable representation of the fitted historical
probability-of-default model.

## Files

- `pd_model_coefficients.csv`: the fitted intercept and feature coefficients,
  with Statsmodels p-values.
- `scorecard.csv`: all scorecard categories, including zero-coefficient
  reference categories and final integer points.
- `model_metadata.json`: the model direction, population, held-out metrics,
  reference categories, score range, library versions, and limitations.

## Intended use

These files make the fitted model and scorecard reviewable without committing
the approximately 513 MB Statsmodels results pickle. They support inspection
and reconstruction of calculations that already use the engineered feature
schema.

They are not a raw-loan scoring package. Raw inputs still require the cleaning,
category grouping, and coarse-classing workflow in notebooks 00 through 03.
The model is an educational historical analysis, not an underwriting,
approval, pricing, or regulatory system.
