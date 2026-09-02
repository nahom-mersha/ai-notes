# Compression Evaluation and k Trade-offs

## Overview

No single metric completely describes the quality of an image-colour compression result.

The project evaluates several properties:

- palette reduction;
- RGB reconstruction error;
- K-means compactness;
- cluster separation;
- runtime;
- visual appearance.

## Unique-colour count

The compressed image should normally use no more than `k` representative colours.

This verifies the palette-reduction claim:

```text
many original colours → k representative colours
```

It does not measure how accurately those colours reconstruct the image.

## RGB reconstruction MSE

Mean squared error measures the average squared difference between the original and reconstructed RGB channel values.

```text
MSE = average of all squared RGB-channel differences
```

Lower MSE means the reconstructed pixels are numerically closer to the original pixels.

RGB MSE is useful, but it is not a perfect perceptual metric. Equal numerical RGB errors do not always appear equally important to the human visual system.

## Inertia

K-means inertia is the total squared distance between every point and its assigned centroid.

```text
inertia = sum of squared assignment distances
```

For image colour compression, inertia and reconstruction MSE use the same pixel differences but aggregate them differently:

- inertia is a total;
- MSE is an average across pixels and RGB channels.

Lower inertia indicates more compact clusters.

## Silhouette score

The silhouette score compares:

- how close a point is to other points in its own cluster;
- how close it is to the nearest different cluster.

Scores closer to 1 indicate compact and well-separated clusters. Scores near 0 indicate overlapping clusters.

Calculating all pairwise distances for every image pixel would be expensive, so the project uses a reproducible sample of 5,000 pixels.

## Runtime and visual inspection

Runtime measures computational cost, while visual inspection reveals effects that a single numerical score may miss.

A useful result therefore needs both:

```text
numerical measurements + visual comparison
```

## Experiment across values of `k`

The project tested `k = 4, 8, 16, 32` with the same image, seed, K-means++ initialization, iteration limit, and silhouette-sampling procedure.

| k | RGB MSE | Silhouette | Iterations | Runtime |
| ---: | ---: | ---: | ---: | ---: |
| 4 | 185.61 | 0.5919 | 14 | 6.47 s |
| 8 | 46.88 | 0.5811 | 20 | 17.79 s |
| 16 | 15.03 | 0.4817 | 20 | 27.93 s |
| 32 | 6.87 | 0.4109 | 20 | 52.24 s |

## Interpreting the result

As `k` increased:

- the image had more representative colours;
- reconstruction MSE decreased;
- inertia decreased;
- runtime increased;
- the silhouette score decreased.

The decreasing silhouette score does not mean that the larger palettes reconstructed the image worse.

The metrics answer different questions:

| Metric | Main question |
| --- | --- |
| Unique colours | How much was the palette reduced? |
| MSE | How closely were RGB values reconstructed? |
| Inertia | How compact are assignments around centroids? |
| Silhouette | How separated are the clusters? |
| Runtime | How expensive was the calculation? |
| Visual comparison | Does the result preserve useful visible detail? |

At larger values of `k`, K-means creates more fine-grained colour groups. These groups can be closer to one another in RGB space, lowering silhouette even while reconstruction improves.

## Selecting `k`

There is no universally best `k`.

A smaller `k` gives:

- stronger palette reduction;
- faster execution;
- fewer preserved colour details.

A larger `k` gives:

- lower reconstruction error;
- more preserved detail;
- less aggressive colour reduction;
- higher computational cost.

Choosing `k` is therefore a quality-versus-compression trade-off rather than a search for the largest possible value.

## Key takeaway

Model evaluation depends on the problem objective. A clustering with the highest silhouette score is not automatically the best image compressor, because compression quality and cluster separation are not the same objective.

## Related notes

- [K-Means from Scratch and Initialization](K-Means%20from%20Scratch%20and%20Initialization.md)
- [Image Data and Colour Quantization](Image%20Data%20and%20Colour%20Quantization.md)
- [Scikit-Learn Comparison, Complexity, and Interactive Delivery](Scikit-Learn%20Comparison,%20Complexity,%20and%20Interactive%20Delivery.md)