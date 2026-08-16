# Cross-Validation, Regularization, and Model Selection

## Overview

The professional model comparison uses a fixed train-test split and five-fold cross-validation inside the training set.

```text
Full dataset
├── Training set
│   └── Cross-validation for model and hyperparameter decisions
└── Test set
    └── Final evaluation after the decisions are made
```

The test set must remain outside cross-validation so it does not influence model selection.

## What cross-validation does

With five folds, the model trains five times. Each fold becomes the validation data once, while the other four folds are used for training.

The validation scores are averaged to provide a more stable comparison than one temporary validation split.

Cross-validation helps answer two different questions:

- Which model family should be used?
- Which hyperparameter settings work best?

Model coefficients are **parameters** learned during fitting. Settings chosen before fitting, such as Ridge `alpha`, are **hyperparameters**.

## Ridge regression

Ridge uses the linear-regression prediction equation but adds an L2 penalty:

```text
squared-error loss + alpha × sum(coefficient²)
```

The penalty discourages unnecessarily large coefficients.

- A very small `alpha` behaves similarly to ordinary linear regression.
- A moderate value can stabilize correlated features and reduce variance.
- An excessively large value can shrink useful effects and cause underfitting.

Among the tested values `0.01`, `0.1`, `1`, `10`, and `100`, `alpha=10` achieved the lowest cross-validation MAE.

## Model comparison

| Model | Mean CV MAE | Test MAE | Test RMSE | Test R² |
| --- | ---: | ---: | ---: | ---: |
| Mean baseline | — | €685.86 | €988.72 | -0.003 |
| Linear Regression | €312.99 | €310.23 | €478.98 | 0.765 |
| Ridge (`alpha=10`) | €311.09 | €308.84 | €479.18 | 0.764 |
| Decision Tree | €330.79 | €318.90 | €495.94 | 0.748 |

All learned models substantially outperformed the mean baseline.

Ridge was selected because MAE was the predefined primary metric and Ridge achieved the lowest cross-validation and test MAE. Ordinary linear regression was extremely close and had marginally better RMSE and R², so Ridge should be described as a reasoned tie-break rather than a dramatically superior model.

The decision tree provided a nonlinear comparison but performed worse for the current features and tuning grid.

## Bias and variance

Regularization illustrates the bias-variance trade-off. Too little constraint can allow unstable patterns, while too much constraint can make the model too simple. Cross-validation helps choose a useful balance without repeatedly consulting the final test set.

## Key takeaway

Model selection should follow a predefined metric, leakage-safe cross-validation, and an honest interpretation of small performance differences.

## Related notes

- [Regression Metrics and Baselines](Regression%20Metrics%20and%20Baselines.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)
- [Error Analysis and Feature Effects](Error%20Analysis%20and%20Feature%20Effects.md)