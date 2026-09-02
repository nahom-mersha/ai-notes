# PCA from Scratch and Explained Variance

## Overview

Principal component analysis transforms data into new, ordered directions that capture as much variance as possible.

For an RGB image, each pixel begins with three features:

```text
[R, G, B]
```

PCA transforms these into principal-component coordinates:

```text
[PC1, PC2, PC3]
```

These are not new colour channels. They are coordinates along directions discovered from the variation in the data.

## Why use PCA?

PCA can:

- reduce the number of dimensions;
- reveal dominant patterns of variation;
- project high-dimensional data into two dimensions;
- help visualize clusters;
- measure how much variance is retained after projection.

In this project, RGB has only three dimensions, so PCA is used mainly for understanding and visualization rather than major computational reduction.

## PCA process

The from-scratch implementation follows this sequence:

```text
Data
  ↓
Calculate feature means
  ↓
Centre the data
  ↓
Calculate the covariance matrix
  ↓
Find eigenvalues and eigenvectors
  ↓
Sort directions by eigenvalue
  ↓
Project the centred data
```

## Centring

PCA subtracts each feature’s mean:

```text
centred_data = data - mean
```

This moves the centre of the data to the origin.

Without centring, the covariance calculation and discovered directions could be influenced by the data’s absolute position rather than variation around its centre.

Centring is essential. Standardization is a separate decision.

## Covariance matrix

Covariance describes how features vary together.

For RGB data, the covariance matrix has shape:

```text
(3, 3)
```

Its diagonal contains the variance of each channel. The off-diagonal values describe how pairs of channels vary together.

The covariance matrix is symmetric because:

```text
covariance(R, G) = covariance(G, R)
```

## Eigenvectors and eigenvalues

The project uses `np.linalg.eigh()` because the covariance matrix is symmetric.

The eigendecomposition returns:

- eigenvectors: candidate principal directions;
- eigenvalues: variance captured along those directions.

The eigenvalues and eigenvectors are sorted from largest to smallest eigenvalue:

```text
PC1 → greatest variance
PC2 → second-greatest variance
PC3 → remaining variance
```

The eigenvectors are stored as rows of the component matrix.

## Projection

The data is projected using:

```text
centred_data @ components.T
```

The dot products calculate each sample’s coordinate along every principal direction.

If the input has shape:

```text
(samples, features)
```

and components have shape:

```text
(components, features)
```

then the transformed result has shape:

```text
(samples, components)
```

## Explained variance

Explained variance is the variance captured by a principal component.

The explained-variance ratio is:

```text
component variance / total variance
```

If all components are retained, their explained-variance ratios normally add to 1, or 100%.

If only the first two components are retained, their cumulative ratio may be less than 100%. That remaining percentage represents variation left in the omitted components.

## Project results

PCA was applied to 5,000 sampled RGB pixels.

| Component | Explained variance ratio |
| --- | ---: |
| PC1 | 99.8171% |
| PC2 | 0.1709% |
| PC3 | 0.0120% |

PC1 and PC2 together preserved approximately `99.988%` of the total variance.

Almost all variation in this sample lay along the first principal direction.

## Comparison with scikit-learn

The from-scratch explained-variance values and ratios matched `sklearn.decomposition.PCA`.

Principal-component direction signs can differ between valid implementations:

```text
v and -v
```

describe the same axis. Tests should therefore account for this sign ambiguity when comparing component directions.

## PCA and K-means do different jobs

| Algorithm | Purpose |
| --- | --- |
| K-means | Creates cluster assignments |
| PCA | Finds informative directions and projects data |

The PCA scatter plot is coloured using K-means assignments. PCA does not create those cluster labels.

Changing `k` can change the colours and grouping shown in the visualization because K-means assignments change. The underlying PCA projection does not need to change when it is fitted to the same sampled RGB data.

## PCA and scaling

PCA does not always require standardization.

RGB channels share the same unit and range, so the project centres them without automatically standardizing each channel to unit variance.

With features that have different units or ranges, scaling becomes a modelling decision because large-scale features can dominate the variance PCA discovers.

## SVD alternative

PCA can also be calculated using singular value decomposition.

This project uses covariance-matrix eigendecomposition because it makes the relationship between covariance, eigenvectors, eigenvalues, and explained variance explicit. Professional implementations often use SVD because of its numerical and practical advantages.

## Key takeaway

PCA is not merely a plotting function. It finds an ordered coordinate system in which the first direction captures the greatest variance, the second captures the next independent source of variance, and projection expresses each sample in those new coordinates.

## Related notes

- [Feature Scaling and the Curse of Dimensionality](Feature%20Scaling%20and%20the%20Curse%20of%20Dimensionality.md)
- [K-Means from Scratch and Initialization](K-Means%20from%20Scratch%20and%20Initialization.md)
- [Scikit-Learn Comparison, Complexity, and Interactive Delivery](Scikit-Learn%20Comparison,%20Complexity,%20and%20Interactive%20Delivery.md)