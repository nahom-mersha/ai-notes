# Project 8 — Neural Network From Scratch for Handwritten Digits

An educational NumPy neural network built from scratch for handwritten-digit classification.

This project extends Project 7's scalar automatic-differentiation ideas to arrays, matrices, batches, and a complete trainable neural network.

Link to the codebase repository: https://github.com/nahom-mersha/neural-network-from-scratch

## What the project builds

- MNIST data preparation and normalization
- Dense layers implemented with NumPy
- ReLU and softmax activations
- Cross-entropy loss
- Manual forward propagation and backpropagation
- Mini-batch gradient descent
- Analytical and numerical gradient checking
- Train, validation, and test evaluation
- Confusion-matrix error analysis
- Model saving and loading
- A Streamlit drawing application

## Overall neural-network flow

```text
28 × 28 image
    ↓
784 flattened pixel values
    ↓
Dense layer: 784 → 32
    ↓
ReLU
    ↓
Dense layer: 32 → 10
    ↓
Softmax
    ↓
Probabilities for digits 0–9
```

The forward pass is:

```text
Z1 = X @ W1 + b1
A1 = ReLU(Z1)
Z2 = A1 @ W2 + b2
P  = softmax(Z2)
```

The training loop repeatedly performs:

```text
forward pass → calculate loss → backward pass → update weights and biases
```

## Project 7 connection (automatic-differentiation-engine)

The link for it's repository is: https://github.com/nahom-mersha/automatic-differentiation-engine

The project worked with scalar values and computational graphs:

```text
scalar values → scalar derivatives → reverse-mode backpropagation
```

Project 8 applies the same chain-rule idea to arrays and matrices:

```text
arrays and matrices → vectorized derivatives → neural-network training
```

The central rule is still:

```text
parent.grad += local_derivative × upstream_gradient
```

Matrix multiplication allows the network to perform many of these scalar gradient calculations at the same time.

See [Dense Layers](Dense%20Layers.md) for the detailed connection.

## Results

Final configuration:

```text
Input size:     784
Hidden size:    32
Output size:    10
Epochs:         10
Batch size:     128
Learning rate:  0.1
Random seed:    42
```

Final results:

```text
Training accuracy:   96.38%
Validation accuracy: 95.60%
Test accuracy:       95.78%
```

## Live demo

[Open the Handwritten Digit Classifier](https://neural-network-from-scratch-nahom.streamlit.app/)

The deployed application performs this inference pipeline:

```text
user drawing
→ grayscale conversion
→ color inversion
→ cropping and centering
→ resizing to 28 × 28
→ normalization to [0, 1]
→ flattening to (1, 784)
→ model forward pass
→ predicted digit and confidence
```

## Learning-rate experiment

A controlled XOR experiment compared three learning rates while keeping the dataset, architecture, batch size, epoch count, and random seed fixed.

| Learning rate | Final loss | Final accuracy |
|---:|---:|---:|
| 0.001 | 0.8703 | 75% |
| 0.01 | 0.6267 | 50% |
| 0.1 | 0.0853 | 100% |

This was an illustrative experiment, not a general hyperparameter search. It showed that the learning rate can strongly affect training speed and final performance.

## Installation and usage

```bash
python -m pip install -e ".[dev,app]"
python -m pytest
python -m scripts.train_mnist
python -m streamlit run app.py
```

The trained model is saved to:

```text
artifacts/mnist_model.npz
```

The NumPy archive stores the learned parameters:

```text
dense1_weights
dense1_biases
dense2_weights
dense2_biases
```

The application recreates the model architecture and loads these parameters for inference.

## Limitations

The model performs well on MNIST-style test images but may perform worse on drawings made in the Streamlit canvas. User drawings can differ in stroke thickness, position, scale, and handwriting style.

A convolutional neural network and stronger data augmentation would likely improve robustness to different drawing styles. Those improvements are left for a later PyTorch project.

## Main learning outcome

This project connects neural-network mathematics to a working AI system:

```text
data preparation → vectorized forward pass → manual backpropagation
→ parameter updates → evaluation → model persistence → user-facing prediction
```

This is an AI-assisted learning project. I used ChatGPT to help generate and explain code, then reviewed the implementation, ran tests, explored the underlying concepts, and documented what I learned.
