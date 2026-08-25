# Gradient Checking, Learning Rates, and Scaling

## Why gradient checking matters

The NumPy logistic-regression model calculates analytical gradients using derived mathematical formulas.

A small error in those formulas can cause training to move in the wrong direction even if the code runs without an exception.

Numerical gradient checking provides an independent approximation that can be compared with the analytical result.

## Central finite differences

For one weight, the numerical gradient is approximated by:

```text
gradient ≈
    [loss(weight + epsilon) - loss(weight - epsilon)]
    / (2 × epsilon)
```

This is the familiar slope idea:

```text
slope = rise / run
```

The weight is moved slightly in both directions, the loss is measured, and the local slope is estimated.

The same process can be applied to the bias.

## Analytical versus numerical gradients

The test uses a small controlled dataset and compares:

```text
analytical weight gradients
numerical weight gradients

analytical bias gradient
numerical bias gradient
```

NumPy checks whether the values are sufficiently close:

```python
np.allclose(
    analytical_gradient,
    numerical_gradient,
    rtol=1e-5,
    atol=1e-7,
)
```

The repository includes an automated test for this comparison.

A successful check does not prove every part of the model is correct, but it provides strong evidence that the gradient formulas and implementation agree.

## Learning-rate experiments

The learning rate determines the size of each gradient-descent step.

- A very small learning rate makes progress slowly.
- A suitable learning rate reduces the loss efficiently.
- A very large learning rate can overshoot the useful region or make training unstable.

The correct value depends partly on the scale of the features and the shape of the loss surface.

## Why scaling helps

Customer features can have very different numerical ranges:

```text
SeniorCitizen  -> usually 0 or 1
tenure         -> tens of months
TotalCharges   -> potentially thousands
```

Without scaling, a single learning rate applies to gradients influenced by very different feature magnitudes. Gradient descent may move inefficiently through the loss surface.

Standardization transforms numerical features toward:

```text
mean ≈ 0
standard deviation ≈ 1
```

This often makes optimization more stable and allows one learning rate to work more consistently across features.

## What I learned

I learned to distinguish two questions:

```text
Are the gradients mathematically correct?
Does gradient descent use them effectively?
```

Gradient checking investigates the first question. Learning-rate and scaling experiments investigate the second.

## Key takeaway

Correct gradients are necessary, but optimization behavior also depends on update size, numerical stability, and feature scale.

## Related notes

- [Logistic Regression from Scratch with NumPy](Logistic%20Regression%20from%20Scratch%20with%20NumPy.md)
- [Leakage-Safe Pipelines and Professional Model Comparison](Leakage-Safe%20Pipelines%20and%20Professional%20Model%20Comparison.md)