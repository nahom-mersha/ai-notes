# Gradient Accumulation and Graph Order

## Shared paths

If the same value appears more than once, it can affect the final output through multiple paths:

```python
a = Value(2.0)
y = a * a + a
```

The two uses of `a` produce separate gradient contributions. The correct result is their sum, so backward rules use `+=` rather than assignment.

## Topological sorting

The engine recursively visits a node's parents, records each node once with a `visited` set, and appends a node after visiting its parents. This produces a parent-before-child order.

The final output is the last node in that order. Reversing the list makes the final output run first during backpropagation.

The `visited` set matters when branches share a node: it prevents duplicate processing while still allowing that node's gradient to receive contributions from every branch.

## Resetting gradients

Gradients accumulate by design. Before reusing a graph for another independent backward pass, call:

```python
output.zero_grad()
```

This resets the reachable nodes' gradients to zero.

## Key takeaway

Graph order answers *when* a local rule may run. Accumulation answers *how* multiple paths are combined. Both are required for correct reverse-mode differentiation.
