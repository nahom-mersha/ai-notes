# Gradient Descent, Scaling, and Closed-Form Solutions

## Overview

Gradient descent is an iterative way to find model parameters that reduce the loss. Its behaviour depends strongly on feature scale and the learning rate.

## Why feature scaling matters

The first model uses features with very different numerical ranges:

```text
livingSpace: tens or hundreds
noRooms: usually single digits
```

Because a linear-regression gradient contains `error × feature value`, a larger feature scale can create much larger gradient values. Using one learning rate for very different gradient magnitudes can cause unstable updates.

Standardization transforms each feature using training statistics:

```text
scaled value = (value - training mean) / training standard deviation
```

This places the inputs on comparable numerical scales without claiming that they are equally important.

## Learning-rate behaviour

The learning rate controls the size of each update:

```text
new weight = old weight - learning rate × gradient
```

During the learning experiments, the observed pattern was:

| Learning rate | Behaviour |
| ---: | --- |
| `0.0001` | Stable but slow |
| `0.001` | Stable and faster |
| `0.01` | Stable and much faster |
| `0.1` and `0.3` | Stable and very fast |
| `1.0` | Diverged |

A learning rate that is too small wastes iterations. A suitable value converges efficiently. A value that is too large overshoots the minimum and can make the loss grow.

## Direct solutions

Linear regression can also be solved without iterative gradient updates.

The project compares four approaches:

```text
Gradient descent
→ iterative optimization

Normal equation with inv()
→ direct mathematical formula

np.linalg.pinv()
→ Moore-Penrose pseudoinverse

np.linalg.lstsq()
→ numerical least-squares solver
```

On the normal training matrix, these methods produced essentially the same weights and training MSE.

## Why not always use an inverse?

The normal-equation expression using `inv()` requires an invertible matrix. If columns are linearly dependent, the matrix can be singular and a direct inverse fails.

`pinv()` and `lstsq()` are generally safer because they can handle singular or nearly singular systems more robustly.

Gradient descent remains useful when datasets and parameter counts become too large for a direct matrix solution, or when the model has no convenient closed-form solution.

## Key takeaway

Scaling improves the numerical behaviour of gradient descent, the learning rate controls convergence, and different mathematical solvers can reach the same linear-regression optimum with different trade-offs.

## Related notes

- [Linear Regression from Scratch with NumPy](Linear%20Regression%20from%20Scratch%20with%20NumPy.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)
- [Cross-Validation, Regularization, and Model Selection](Cross-Validation%2C%20Regularization%2C%20and%20Model%20Selection.md)