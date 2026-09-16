# Backpropagation From Loss to Input

## The complete backward path

For the neural network in Project 8, the forward pass is:

```text
X → Dense 1 → ReLU → Dense 2 → Softmax → Cross-Entropy → Loss
```

The backward pass reverses this path:

```text
Loss → Cross-Entropy → Softmax → Dense 2 → ReLU → Dense 1 → Input
```

The purpose of backpropagation is to calculate how much the loss depends on every intermediate value and parameter:

```text
dW1, db1, dW2, db2, dX
```

The same chain-rule idea from Project 7 is used at every step:

```text
parent gradient += local derivative × upstream gradient
```

Project 7 applied this rule to scalar values. Project 8 applies it to vectors, matrices, and batches.

---

## 1. The network and its shapes

For a batch of examples, our convention is:

```text
X  : (batch_size, 784)
W1 : (784, 32)
b1 : (32,)
Z1 : (batch_size, 32)
A1 : (batch_size, 32)

W2 : (32, 10)
b2 : (10,)
Z2 : (batch_size, 10)
P  : (batch_size, 10)
Y  : (batch_size, 10)
```

The forward equations are:

```text
Z1 = X @ W1 + b1
A1 = ReLU(Z1)
Z2 = A1 @ W2 + b2
P  = softmax(Z2)
```

`P` contains predicted probabilities, and `Y` contains one-hot target labels.

---

## 2. From the loss to the probabilities

The multiclass cross-entropy loss is:

```python
loss = -np.mean(
    np.sum(Y * np.log(P), axis=1)
)
```

For one example:

```text
L = -Σᵢ Yᵢ log(Pᵢ)
```

Because `Y` is one-hot, only the correct class contributes to the loss.

For example:

```text
Y = [0, 1]
P = [0.2, 0.8]
L = -log(0.8)
```

The derivative of `-log(P)` is:

```text
∂L/∂P = -1/P
```

Using the one-hot target:

```text
dP = -Y / P
```

Therefore:

```text
dP = -[0, 1] / [0.2, 0.8]
   = [0, -1.25]
```

The negative gradient for the correct class means that increasing its probability would decrease the loss.

Our code calculates the mean loss across the batch. If the batch has `m` examples, every example contributes `1/m` of the total loss. Therefore:

```text
dP = -Y / P / m
```

In code, with safe clipped probabilities:

```python
dP = -targets / clipped_probabilities / batch_size
```

At this stage:

```text
dP = ∂loss / ∂P
```

`dP` has the same shape as `P`.

---

## 3. Why dP is needed even though it disappears later

The forward path contains:

```text
Z2 → softmax → P → cross-entropy → loss
```

Therefore, the backward path must contain:

```text
loss → cross-entropy → P → softmax → Z2
```

Cross-entropy first gives:

```text
dP = ∂loss / ∂P
```

Then softmax passes that gradient to the logits:

```text
dP → softmax derivative → dZ2
```

So `dP` is not ignored. It is an intermediate gradient. When it is combined with the softmax derivatives, the expression simplifies to `P - Y`.

The final implementation uses the combined formula directly:

```python
dZ2 = (probabilities - targets) / batch_size
```

This is called the fused softmax-cross-entropy gradient.

---

## 4. Softmax and Project 7 gradient accumulation

Softmax is different from an elementwise activation such as ReLU. It couples all classes through a shared denominator:

```text
Pᵢ = exp(Zᵢ) / Σⱼ exp(Zⱼ)
```

Changing one logit can change every probability.

For a particular logit `Z[j]`, its gradient receives a contribution from every probability:

```text
dZ[j] = Σᵢ dP[i] × ∂P[i]/∂Z[j]
```

This is exactly Project 7's rule:

```text
parent.grad += local_derivative × upstream_gradient
```

There are multiple paths from `Z[j]` to the loss, so all contributions must be added.

For one example, the softmax derivatives are:

```text
if i = j:  ∂P[i]/∂Z[j] = P[i](1 - P[i])
if i ≠ j:  ∂P[i]/∂Z[j] = -P[i]P[j]
```

The complete collection of these derivatives is called the softmax Jacobian.

After multiplying the local derivatives by the upstream gradients `dP` and summing all paths, softmax and cross-entropy simplify to:

```text
dZ2 = P - Y
```

For a mean batch loss:

```text
dZ2 = (P - Y) / batch_size
```

The final code does not explicitly construct the softmax Jacobian. The mathematics is still present; it has been algebraically simplified and implemented more efficiently.

---

## 5. Backward through Dense 2

Dense 2 performed:

```text
Z2 = A1 @ W2 + b2
```

The incoming gradient is:

```text
dZ2 = ∂loss / ∂Z2
```

Apply the dense-layer formulas:

```text
dW2 = A1ᵀ @ dZ2
db2 = dZ2.sum(axis=0)
dA1 = dZ2 @ W2ᵀ
```

Shapes make the transposes clear:

```text
A1  : (batch_size, 32)
dZ2 : (batch_size, 10)
A1ᵀ : (32, batch_size)

dW2 = A1ᵀ @ dZ2
     : (32, 10)
```

The result has the same shape as `W2`.

For one scalar connection, this is simply:

```text
gradient of a weight
= input × gradient of its output
```

`A1ᵀ @ dZ2` performs all those scalar multiplications and accumulations for the whole batch at once.

The bias is shared across examples, so its gradient sums down the rows:

```python
db2 = dZ2.sum(axis=0)
```

`axis=0` means sum over examples:

```text
(batch_size, 10) → (10,)
```

Finally, Dense 2 passes the gradient to the previous activation:

```text
dA1 = dZ2 @ W2ᵀ
```

```text
(batch_size, 10) @ (10, 32)
= (batch_size, 32)
```

---

## 6. Backward through ReLU

The forward ReLU operation is:

```text
A1 = ReLU(Z1)
```

For each value:

```text
ReLU(z) = max(0, z)
```

Its derivative is:

```text
ReLU'(z) = 1 if z > 0
           0 if z ≤ 0
```

The upstream gradient is `dA1`, received from Dense 2. The local derivative is the ReLU mask:

```python
dZ1 = dA1 * (Z1 > 0)
```

This follows Project 7's rule:

```text
gradient passed backward
= upstream gradient × local derivative
```

The mask controls which gradients pass:

```text
Z1 > 0 → gradient passes through
Z1 ≤ 0 → gradient becomes zero
```

`dZ1` has the same shape as `Z1`:

```text
dZ1 : (batch_size, 32)
```

---

## 7. Backward through Dense 1

Dense 1 performed:

```text
Z1 = X @ W1 + b1
```

The incoming gradient is:

```text
dZ1 = ∂loss / ∂Z1
```

Apply the same dense-layer formulas:

```text
dW1 = Xᵀ @ dZ1
db1 = dZ1.sum(axis=0)
dX  = dZ1 @ W1ᵀ
```

Shapes:

```text
X   : (batch_size, 784)
dZ1 : (batch_size, 32)
Xᵀ  : (784, batch_size)

dW1 = Xᵀ @ dZ1 : (784, 32)
db1 = dZ1.sum(axis=0) : (32,)
dX  = dZ1 @ W1ᵀ : (batch_size, 784)
```

`dX` is the gradient of the loss with respect to the input pixels. We do not update the pixels, but calculating `dX` completes the backward chain.

---

## 8. Complete backward pass in code form

Conceptually, the model's backward pass is:

```python
# Loss → softmax/cross-entropy
dZ2 = (probabilities - targets) / batch_size

# Dense 2
dA1 = dense2.backward(dZ2)

# ReLU
dZ1 = dA1 * (Z1 > 0)

# Dense 1
dX = dense1.backward(dZ1)
```

Inside each dense layer:

```python
def backward(self, output_gradient):
    self.weights_gradient = self.inputs.T @ output_gradient
    self.biases_gradient = output_gradient.sum(axis=0)
    return output_gradient @ self.weights.T
```

The names mean:

```text
output_gradient = gradient arriving from the next layer
weights_gradient = gradient for this layer's weights
biases_gradient  = gradient for this layer's biases
returned value   = gradient passed to the previous layer
```

---

## 9. Complete backward-flow summary

```text
Loss
  ↓
Cross-entropy
  dP = -Y / P / batch_size
  ↓
Softmax
  dZ2 = (P - Y) / batch_size
  ↓
Dense 2
  dW2 = A1ᵀ @ dZ2
  db2 = dZ2.sum(axis=0)
  dA1 = dZ2 @ W2ᵀ
  ↓
ReLU
  dZ1 = dA1 * (Z1 > 0)
  ↓
Dense 1
  dW1 = Xᵀ @ dZ1
  db1 = dZ1.sum(axis=0)
  dX  = dZ1 @ W1ᵀ
  ↓
Input
```

The compact implementation skips explicit `dP` because softmax and cross-entropy are fused:

```text
cross-entropy gradient + softmax derivative
→ (P - Y) / batch_size
```

---

## 10. Connection to Project 7

Project 7 used a scalar computational graph:

```text
x → multiply → add → activation → loss
```

Each scalar node stored a gradient and propagated it backward:

```text
parent.grad += local_derivative × upstream_gradient
```

Project 8 uses the same graph idea:

```text
X → Dense 1 → ReLU → Dense 2 → Softmax → Loss
```

The difference is that one NumPy expression represents many scalar operations.

For example:

```python
dW2 = A1.T @ dZ2
```

is a vectorized form of many calculations where each weight receives:

```text
input value × output gradient
```

For softmax, one logit influences multiple probabilities. Therefore, its gradient receives multiple contributions:

```text
dZ[j] = Σᵢ dP[i] × ∂P[i]/∂Z[j]
```

That summation is the matrix-scale version of Project 7's repeated `parent.grad += ...` behavior.

```text
Project 7:
scalar operations → scalar gradient accumulation

Project 8:
vector/matrix operations → vectorized gradient accumulation
```

---

## 11. Memory rules

### Rule 1: Backpropagation follows the reverse forward path

```text
Forward:  X → Dense → ReLU → Dense → Softmax → Loss
Backward: Loss → Softmax/Cross-Entropy → Dense → ReLU → Dense → X
```

### Rule 2: Every gradient has the shape of its value

```text
Z1 and dZ1 have the same shape
A1 and dA1 have the same shape
W1 and dW1 have the same shape
b1 and db1 have the same shape
```

### Rule 3: A dense layer sends gradients in three directions

```text
dW = inputᵀ @ output-gradient
db = output-gradient summed over examples
dX = output-gradient @ weightsᵀ
```

### Rule 4: `axis=0` sums over examples in our convention

Rows are examples, so:

```python
db = dZ.sum(axis=0)
```

combines the gradient contribution from every example for each neuron.

### Rule 5: ReLU gates the gradient

```text
positive forward value → gradient passes
zero/negative forward value → gradient blocked
```

### Rule 6: `dP` is intermediate; `dZ2` is what Dense 2 needs

```text
dP  = gradient with respect to probabilities
dZ2 = gradient with respect to logits
```

The final implementation uses:

```python
dZ2 = (P - Y) / batch_size
```

because it combines both steps safely and efficiently.

### Rule 7: Transposes come from the chain rule and shapes

For our row-batch convention:

```text
dW = inputᵀ @ output-gradient
dX = output-gradient @ Wᵀ
```

Another implementation may orient its matrices differently, so transpose locations can change. The underlying chain rule does not change.

---

## Final understanding

Backpropagation is not a collection of unrelated formulas to memorize.

It is one repeated process:

```text
receive an upstream gradient
→ apply the local derivative
→ calculate parameter gradients
→ pass a gradient to the previous operation
```

For the complete Project 8 network:

```text
loss
→ probability gradient
→ logit gradient
→ Dense 2 gradients
→ ReLU gradient
→ Dense 1 gradients
→ input gradient
```

The formulas are the vectorized form of the same chain rule implemented with scalar nodes in Project 7.

