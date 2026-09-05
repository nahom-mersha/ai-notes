# Automatic Differentiation Engine

Notes from Project 7 of my AI Engineering roadmap.

This project explains and validates a small scalar automatic-differentiation engine. It represents values as nodes in a computational graph, records the operations that created them, and propagates gradients backward using the chain rule.

Repository: [automatic-differentiation-engine](https://github.com/nahom-mersha/automatic-differentiation-engine)

## Source of inspiration

The project is independently developed for learning and is strongly informed by Andrej Karpathy's educational [`micrograd`](https://github.com/karpathy/micrograd) project and lecture, [The spelled-out intro to neural networks and backpropagation](https://www.youtube.com/watch?v=VMj-3S1tku0).

The central idea is the same: use scalar `Value` nodes, record operations, attach local backward rules, and apply the chain rule through the graph. The explanations, tests, validation, and project organization are my own learning work.

## What I learned

- A computational graph turns an expression into connected value and operation nodes.
- Each operation has a forward calculation and a local backward rule.
- Backpropagation multiplies local derivatives by upstream gradients.
- Gradients must accumulate when a value influences the output through multiple paths.
- Reverse topological order ensures every node receives all downstream contributions before it propagates them.
- Numerical finite differences and PyTorch autograd provide independent validation.

## Notes

- [Scalar Value Objects and Local Backward Rules](Scalar%20Value%20Objects%20and%20Local%20Backward%20Rules.md)

- [The Chain Rule and Backpropagation](The%20Chain%20Rule%20and%20Backpropagation.md)

- [Gradient Accumulation and Graph Order](Gradient%20Accumulation%20and%20Graph%20Order.md)

- [Finite Differences and PyTorch Validation](Finite%20Differences%20and%20PyTorch%20Validation.md)

## Verified implementation

The engine supports scalar `Value` objects, arithmetic operations, powers, division, ReLU, `tanh`, graph construction, topological sorting, reverse-mode backpropagation, gradient resetting, finite-difference checking, and comparison with PyTorch autograd.

The test suite covers forward values, local derivatives, shared nodes, complete backward passes, activations, hand calculations, numerical derivatives, and PyTorch comparisons.

## Key takeaway

Automatic differentiation is not symbolic algebra and not an approximation. It evaluates ordinary operations while recording enough structure to calculate exact derivatives efficiently through the chain rule.
