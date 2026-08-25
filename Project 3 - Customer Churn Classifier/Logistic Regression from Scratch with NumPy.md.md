# Logistic Regression from Scratch with NumPy

## Overview

The learning-focused NumPy model exposes the mechanics of logistic regression instead of treating it as a black box.

I learned how a linear score, sigmoid function, binary cross-entropy loss, gradients, and parameter updates connect to produce churn probabilities.

## Linear score

Logistic regression begins with a weighted linear score:

```text
z = Xw + b
```

Where:

- `X` contains the customer features;
- `w` contains one learned weight per feature;
- `b` is the bias;
- `z` contains one raw score per customer.

The raw score can be any real number, so it cannot yet be interpreted as a probability.

## Sigmoid

The sigmoid function maps each raw score to a value between zero and one:

```text
p = 1 / (1 + exp(-z))
```

Interpretation:

```text
large negative z -> probability near 0
z near 0         -> probability near 0.5
large positive z -> probability near 1
```

The implementation uses different equivalent calculations for positive and negative inputs to reduce numerical overflow.

## Binary cross-entropy

Binary cross-entropy measures the quality of the predicted probabilities:

```text
loss = -mean(
    y * log(p)
    + (1 - y) * log(1 - p)
)
```

It strongly penalizes confident predictions that are wrong.

Probabilities are clipped slightly away from exactly zero and one before taking logarithms because:

```text
log(0)
```

is undefined.

## Vectorized gradients

The prediction error is:

```python
error = probabilities - y
```

The gradients are calculated for all samples together:

```python
dw = (X.T @ error) / number_of_samples
db = mean(error)
```

`X.T @ error` combines each feature with the prediction errors and produces one gradient per weight.

## Gradient descent

Training repeatedly updates the parameters:

```python
weights = weights - learning_rate * weight_gradient
bias = bias - learning_rate * bias_gradient
```

The learning rate controls the size of each update.

The training loss decreased from:

```text
0.6931 -> 0.4144
```

This showed that the optimization process learned a useful decision boundary.

## Probability and label prediction

The model first returns probabilities:

```python
probabilities = sigmoid(X @ weights + bias)
```

Labels are produced separately:

```python
predictions = (probabilities >= threshold).astype(int)
```

This separation allows the same trained model to operate under different decision policies.

## What I learned

I learned that logistic regression is a sequence of understandable operations:

```text
features
-> linear scores
-> sigmoid probabilities
-> binary cross-entropy loss
-> gradients
-> parameter updates
```

## Key takeaway

Logistic regression is a linear model for the log-odds of a class, but its sigmoid output makes it useful for estimating binary-class probabilities.

## Related notes

- [Gradient Checking, Learning Rates, and Scaling](Gradient%20Checking%2C%20Learning%20Rates%2C%20and%20Scaling.md)
- [Classification Metrics, Thresholds, and Class Imbalance](Classification%20Metrics%2C%20Thresholds%2C%20and%20Class%20Imbalance.md)