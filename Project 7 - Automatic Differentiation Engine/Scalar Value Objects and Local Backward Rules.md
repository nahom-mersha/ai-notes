# Scalar Value Objects and Local Backward Rules

## Overview

The engine uses one `Value` object for one scalar number. It stores:

- `data`: the forward numerical value;
- `grad`: the derivative of the chosen final output with respect to this value;
- `_prev`: the immediate input nodes;
- `_op`: the operation that produced the node;
- `_backward`: a function containing the local derivative rule.

For example, if `c = a + b`, then `a` and `b` are the parents of `c`.

## Forward and backward roles

An operation first creates its output node:

```python
out = Value(self.data + other.data, (self, other), "+")
```

It then attaches the rule needed later during backpropagation:

```python
def _backward() -> None:
    self.grad += out.grad
    other.grad += out.grad

out._backward = _backward
```

The function is stored rather than immediately executed because the complete graph must be built first. Later, `backward()` calls each node's stored rule in the correct order.

## Local and upstream gradients

- Local derivative: derivative of the current operation's output with respect to one immediate input.
- Upstream gradient: derivative of the final output with respect to the current operation's output.
- Parent contribution: local derivative multiplied by upstream gradient.

For multiplication, `out = a * b`:

```text
contribution to a.grad = b * out.grad
contribution to b.grad = a * out.grad
```

The `+=` preserves contributions arriving through different paths.

## Key takeaway

Each operation supplies its own local derivative. The engine reuses the same chain-rule pattern to combine that derivative with the gradient already flowing from the output.
