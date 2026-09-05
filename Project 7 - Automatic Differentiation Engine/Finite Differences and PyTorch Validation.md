# Finite Differences and PyTorch Validation

## Why validate?

A derivative implementation can look mathematically correct and still contain a coding error. The project validates the engine in three independent ways:

1. hand-calculated derivatives;
2. central finite differences;
3. PyTorch autograd.

## Finite differences

For a scalar function `f`, the numerical derivative is approximated by:

```text
f'(x) approximately (f(x + epsilon) - f(x - epsilon)) / (2 * epsilon)
```

Each perturbed input must rebuild the forward computation:

```python
def function(x: float) -> float:
    value = Value(x)
    return (value * value + 2 * value).tanh().data
```

Calling the function with `x + epsilon` and `x - epsilon` creates fresh graphs with fresh forward values.

## PyTorch comparison

PyTorch is used only as an optional validation dependency. It is not part of the scalar engine itself. The same scalar expression is evaluated by both systems, and the resulting values and gradients are compared.

The comparison uses matching 64-bit precision so that tiny floating-point differences do not create false failures.

## Key takeaway

Validation checks different failure modes: hand calculations test understanding, finite differences test numerical behavior, and PyTorch provides a mature external reference.
