# Regression Metrics and Baselines

## Overview

A regression model needs both a training objective and fair evaluation metrics.

These ideas are related, but they answer different questions:

```text
Loss function
→ What should training minimize?

Evaluation metric
→ How well does the trained model perform?
```

## Mean-squared error

The NumPy linear-regression model uses mean-squared error as its loss function:

```text
MSE = mean((prediction - actual)²)
```

Squaring prevents positive and negative errors from cancelling. It also gives large errors more influence during training.

## Evaluation metrics

### Mean absolute error

```text
MAE = mean(abs(actual - prediction))
```

MAE is easy to interpret because it uses the target unit. An MAE of €300 means the predictions are approximately €300 away from the recorded rent on average.

### Root mean squared error

```text
RMSE = sqrt(mean((actual - prediction)²))
```

RMSE gives large mistakes more influence than MAE. A much higher RMSE than MAE often suggests that some predictions have very large errors.

### R²

R² compares model performance with a constant mean prediction. A value near zero indicates little improvement over that reference, while a negative value is worse than it.

## Mean-price baseline

Before comparing learned models, the project creates a simple baseline:

```text
Always predict the mean rent from the training set.
```

The mean must come from `y_train`, not `y_test`. Using the test mean would let information from the evaluation data influence the predictor.

The finalized mean baseline produced:

| Metric | Result |
| --- | ---: |
| Test MAE | €685.86 |
| Test RMSE | €988.72 |
| Test R² | -0.003 |

The learned models performed substantially better, showing that the property features contain useful predictive information.

## Loss versus metric

The same mathematical quantity can sometimes serve both roles. For example, MSE can guide training and also be reported for evaluation. The distinction depends on how the quantity is being used.

In this project:

```text
Training objective
→ MSE

Model evaluation
→ MAE, RMSE, and R²
```

## Key takeaway

A model score only becomes meaningful when it is compared with a simple baseline and interpreted using metrics that match the practical problem.

## Related notes

- [Linear Regression from Scratch with NumPy](Linear%20Regression%20from%20Scratch%20with%20NumPy.md)
- [Cross-Validation, Regularization, and Model Selection](Cross-Validation%2C%20Regularization%2C%20and%20Model%20Selection.md)
- [Error Analysis and Feature Effects](Error%20Analysis%20and%20Feature%20Effects.md)