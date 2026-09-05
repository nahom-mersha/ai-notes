# The Chain Rule and Backpropagation

## The central idea

For a sequence such as `a -> c -> y`, the chain rule says:

```text
dy/da = dy/dc * dc/da
```

The engine calculates this locally at every edge and passes the result toward the inputs.

## Backpropagation

`backward()` starts at the chosen final output:

```python
self.grad = 1.0
```

This represents the derivative of the output with respect to itself. It then calls each node's local `_backward` function in reverse topological order.

For an operation `out = f(a)`, the parent receives:

```text
a.grad += local_derivative * out.grad
```

The multiplication is the chain rule. The addition is gradient accumulation.

## Why reverse topological order?

Parents must be processed after all relevant child branches have contributed their gradients. Reversing the order produced by a parent-first recursive traversal gives the correct dependency order for backward propagation.

## Activation functions

ReLU and `tanh` are ordinary graph operations with special local rules:

- ReLU passes the upstream gradient when its input is positive and passes zero otherwise.
- `tanh` uses the local derivative `1 - out.data ** 2`.

They are useful in neural networks because they introduce nonlinearity, while still participating in backpropagation like every other operation.

## Key takeaway

Backpropagation is organized chain-rule computation. It is not a separate kind of calculus; it is an efficient way to apply the chain rule across a graph.
