# Linear Regression from Scratch with NumPy

## Overview

The learning-focused NumPy model makes the mechanics of linear regression visible instead of treating the algorithm as a black box.

I learned how predictions, loss, gradients, and parameter updates connect to one another.

## Prediction equation

For multiple features, linear regression predicts:

```text
prediction = bias
           + weight_1 × feature_1
           + weight_2 × feature_2
           + ...
```

For the first rental model:

```text
predicted rent = bias
               + weight_1 × livingSpace
               + weight_2 × noRooms
```

An intercept column of ones is added to the feature matrix so the bias can be included in the same matrix operation as the other weights.

Each row therefore looks like:

```text
[1, scaled_living_space, scaled_number_of_rooms]
```

## Vectorized predictions

Predictions for every training row can be calculated together:

```python
predictions = X_train_design @ weights
```

If the design matrix has shape `(n, 3)` and the weights have shape `(3,)`, the result contains one prediction for each of the `n` apartments.

Vectorization avoids manually looping through every row and uses NumPy's optimized array operations.

## Errors and loss

The model first calculates:

```python
errors = predictions - y_train
```

Mean-squared error summarizes those mistakes:

```python
mse = np.mean(errors**2)
```

Training tries to find the weight values that minimize this loss.

## Vectorized gradient

For all weights at once, the gradient is:

```python
gradient = (2 / n) * (X_train_design.T @ errors)
```

The gradient combines two ideas:

```text
How wrong was the prediction?
×
How strongly did this feature affect it?
```

The transpose aligns each feature column with all prediction errors, producing one partial derivative for each weight.

## Weight update

Gradient descent repeatedly updates the parameters:

```python
weights = weights - learning_rate * gradient
```

Subtracting the gradient moves the weights in the direction that reduces the loss.

## Key takeaway

Linear regression is a chain of understandable operations: matrix multiplication creates predictions, predictions create errors, errors create a loss, and derivatives show how to update the weights.

## Related notes

- [Regression Metrics and Baselines](Regression%20Metrics%20and%20Baselines.md)
- [Gradient Descent, Scaling, and Closed-Form Solutions](Gradient%20Descent%2C%20Scaling%2C%20and%20Closed-Form%20Solutions.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)