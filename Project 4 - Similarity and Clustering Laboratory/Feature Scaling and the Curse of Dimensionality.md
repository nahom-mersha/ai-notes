# Feature Scaling and the Curse of Dimensionality

## Overview

Distance-based algorithms are sensitive to the numerical representation of their features.

A feature with a much larger numeric range can dominate the distance calculation even when it is not more important for the problem.

## RGB-only features

For normal RGB pixels:

```text
R: 0–255
G: 0–255
B: 0–255
```

All three channels already use the same unit and numeric range. Scaling them together by the same constant changes the size of the distances but not the relative neighbour ordering.

## Adding spatial position

Suppose the feature vector is extended to:

```text
[R, G, B, x, y]
```

If position is normalized while RGB is unchanged:

```text
RGB: 0–255
x, y: 0–1
```

then RGB differences will usually dominate the distance.

A difference of 50 in an RGB channel is much larger numerically than the maximum possible difference of 1 in a normalized coordinate.

If position is brought to a scale closer to RGB, both colour and location can influence similarity.

## Scaling changes meaning

Scaling is not automatically an improvement. It changes the relative importance of the features.

For pure colour compression, adding strongly weighted position features could make K-means group pixels partly by image location instead of only by colour.

The important question is therefore:

> What should “similar” mean for this task?

For colour quantization, colour may be the only desired feature. For a spatial segmentation task, position may deserve meaningful weight.

## Curse of dimensionality

The curse of dimensionality describes problems that appear when data contains many dimensions.

This project investigates distance concentration by starting with standardized RGB features and adding irrelevant random dimensions.

As dimensions are added, absolute distances generally grow. More importantly, the nearest and farthest distances become more similar relative to their overall size.

Example:

```text
Low dimensions:
nearest  = 2
farthest = 10
ratio    = 0.20

High dimensions:
nearest  = 13
farthest = 16
ratio    = 0.81
```

As the ratio approaches 1:

```text
nearest distance ≈ farthest distance
```

The nearest neighbour becomes less clearly “near.”

## Why RGB was standardized

The experiment standardized RGB to approximately mean 0 and standard deviation 1 before adding standard-normal noise.

Without this step:

```text
RGB values: approximately 0–255
noise:      approximately -1 to 1
```

RGB would dominate, and the experiment would mostly demonstrate a scaling imbalance.

Putting RGB and noise on comparable scales isolates the effect of adding irrelevant dimensions.

## Experimental results

The experiment used 250 pixels and measured distance concentration, KNN stability, runtime, and estimated temporary memory.

| Dimensions | Distance ratio | KNN stability | Runtime | Temporary memory |
| ---: | ---: | ---: | ---: | ---: |
| 3 | 0.007 | 1.000 | 1.72 ms | 1.43 MB |
| 5 | 0.074 | 0.123 | 2.47 ms | 2.38 MB |
| 10 | 0.251 | 0.060 | 5.71 ms | 4.77 MB |
| 20 | 0.421 | 0.050 | 7.44 ms | 9.54 MB |
| 50 | 0.604 | 0.046 | 16.65 ms | 23.84 MB |
| 100 | 0.705 | 0.028 | 33.87 ms | 47.68 MB |

## Interpretation

From 3 to 100 dimensions:

- the distance ratio increased from `0.007` to `0.705`;
- KNN stability fell from `1.000` to `0.028`;
- runtime increased from `1.72` to `33.87` milliseconds;
- temporary memory increased from `1.43` to `47.68` MB.

At 100 dimensions, only about 2.8% of the original five-neighbour relationships remained on average.

The irrelevant features changed which points appeared similar, weakened distance contrast, and increased computational cost.

## Practical responses

Possible responses to high-dimensional distance problems include:

- removing irrelevant features;
- selecting more informative features;
- applying dimensionality reduction;
- reconsidering the distance metric;
- using domain knowledge to define a better representation.

## Key takeaway

Distance has no task-independent meaning. Feature selection, feature scale, and dimensionality determine which points an algorithm considers similar.

## Related notes

- [Distance Metrics, Broadcasting, and KNN](Distance%20Metrics,%20Broadcasting,%20and%20KNN.md)
- [PCA from Scratch and Explained Variance](PCA%20from%20Scratch%20and%20Explained%20Variance.md)
- [Compression Evaluation and k Trade-offs](Compression%20Evaluation%20and%20k%20Trade-offs.md)