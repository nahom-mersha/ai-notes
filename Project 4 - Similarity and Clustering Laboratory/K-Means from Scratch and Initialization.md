# K-Means from Scratch and Initialization

## Overview

K-means is an unsupervised-learning algorithm that divides data into `k` clusters.

For image colour compression, every pixel is an RGB point and each cluster centroid becomes one representative palette colour.

## Main K-means loop

K-means repeatedly performs two main operations:

```text
Assign each pixel to its nearest centroid
                 ↓
Update each centroid to the mean of its cluster
                 ↓
Repeat
```

The complete process is:

1. Select `k` initial centroids.
2. Assign each pixel to its nearest centroid.
3. Calculate the mean colour of every cluster.
4. Move each centroid to that mean.
5. Measure centroid movement.
6. Repeat until the movement is small enough or the iteration limit is reached.

## Assignment step

The assignment step calculates the Euclidean distance between every pixel and every centroid.

Each pixel receives the index of its nearest centroid:

```text
pixel → nearest centroid → cluster assignment
```

Standard K-means is connected to squared Euclidean distance and the mean update. Replacing Euclidean distance with Manhattan distance would define a different clustering objective rather than a simple metric option for standard K-means.

## Update step

For each cluster, the new centroid is the mean of the assigned pixels.

For example:

```text
[240, 20, 20]
[250, 30, 25]
```

produce the mean colour:

```text
[245, 25, 22.5]
```

This moves the centroid toward the centre of its assigned RGB colours.

If a cluster becomes empty, the project retains its previous centroid rather than calculating a mean from an empty array.

## Convergence

After every update, the implementation measures the overall movement of the centroids.

Training stops when:

```text
centroid movement <= tolerance
```

or when the maximum number of iterations is reached.

The implementation records inertia after each iteration. On a normal K-means update, inertia should not increase unexpectedly.

## Random initialization

Random initialization selects `k` different input pixels as the starting centroids.

This is simple, but poor initial choices can:

- place several centroids in one region;
- miss another important region;
- produce higher final reconstruction error;
- lead to different local solutions across random seeds.

K-means is stochastic because its result can depend on the randomly selected starting points.

## K-means++ initialization

K-means++ selects starting centroids more carefully.

It:

1. Selects the first centroid randomly.
2. Measures every point’s squared distance to its nearest selected centroid.
3. Gives distant points a higher probability of becoming the next centroid.
4. Repeats until `k` centroids are selected.

This spreads the initial centroids across the data and reduces the chance of starting with several nearly identical representatives.

## Initialization experiment

The project compared random initialization with K-means++ using:

- the same image;
- `k = 8`;
- seeds 1 through 5;
- a maximum of 20 iterations.

| Metric | Random | K-means++ |
| --- | ---: | ---: |
| Mean final inertia | 166.41 million | 140.96 million |
| Mean RGB MSE | 60.73 | 51.44 |
| Inertia range | 133.70–180.95 million | 130.89–145.83 million |
| MSE range | 48.79–66.03 | 47.77–53.22 |
| Mean runtime | 14.41 s | 13.31 s |

K-means++ achieved lower inertia and MSE in four of the five seed comparisons. Its results also varied less between seeds.

Every run reached the 20-iteration limit, so the experiment does not prove that K-means++ converged faster.

## Local solutions

K-means does not guarantee the globally best clustering. Different initial centroids can lead to different final local solutions.

That is why initialization, repeated runs, controlled random seeds, and honest reporting matter.

## Key takeaway

The assignment and update rules define K-means, but initialization strongly influences where that iterative process begins. K-means++ is useful because it chooses more widely separated starting points instead of relying on a completely random palette.

## Related notes

- [Compression Evaluation and k Trade-offs](Compression%20Evaluation%20and%20k%20Trade-offs.md)
- [Distance Metrics, Broadcasting, and KNN](Distance%20Metrics,%20Broadcasting,%20and%20KNN.md)
- [PCA from Scratch and Explained Variance](PCA%20from%20Scratch%20and%20Explained%20Variance.md)