# Scikit-Learn Comparison, Complexity, and Interactive Delivery

## Overview

From-scratch implementations expose the mathematical steps of an algorithm. Professional libraries provide optimized, tested, and feature-complete implementations for practical work.

This project uses both:

```text
From scratch → understanding and verification
Scikit-learn → professional implementation and comparison
```

## What was compared

The project compares its implementations with:

- `sklearn.neighbors.NearestNeighbors`;
- `sklearn.cluster.KMeans`;
- `sklearn.decomposition.PCA`.

The comparisons examine correctness, behaviour, and runtime rather than expecting every internal result to be identical.

## KNN comparison

The KNN neighbour distances matched scikit-learn.

The exact neighbour indices did not always match because multiple RGB pixels can have equal distances. Different implementations may return equally valid tied neighbours in different orders.

This means that exact index equality is not always the correct definition of agreement.

## K-means comparison

The from-scratch and scikit-learn K-means implementations produced similar inertia:

| Implementation | Runtime | Inertia |
| --- | ---: | ---: |
| From scratch | 0.0352 s | 709,506.80 |
| Scikit-learn | 0.0313 s | 709,867.48 |

Exact equality is not expected because initialization and optimization details can lead to different local solutions.

Cluster label numbers can also be permuted. One implementation may call a group cluster 0 while another calls the same group cluster 2.

## PCA comparison

The from-scratch PCA implementation matched scikit-learn for:

- explained variance;
- explained-variance ratios;
- principal directions, after accounting for sign ambiguity.

This verified the covariance, eigendecomposition, sorting, and projection logic.

## Runtime results

The small benchmark recorded:

| Algorithm | Implementation | Runtime |
| --- | --- | ---: |
| KNN | From scratch | 0.0064 s |
| KNN | Scikit-learn | 0.0271 s |
| K-means | From scratch | 0.0352 s |
| K-means | Scikit-learn | 0.0313 s |

The from-scratch KNN result in this small test does not prove that it is generally faster.

Runtime depends on:

- dataset size;
- dimensionality;
- repeated-call overhead;
- implementation details;
- optimized code paths;
- system load;
- timing procedure.

Professional libraries are designed for broader workloads and provide capabilities that are not represented by one small benchmark.

## Pairwise-distance complexity

For:

- `q` queries;
- `r` references;
- `d` dimensions;

pairwise distance calculation requires approximately:

```text
Time:   O(q × r × d)
Memory: O(q × r × d)
```

The temporary memory comes from the broadcasted difference array.

## Brute-force KNN complexity

Brute-force KNN calculates all distances and then ranks the references.

```text
Distance calculation: O(q × r × d)
Sorting:              O(q × r log r)
```

This is clear and suitable for small experiments, but it does not scale well to large reference collections.

## K-means complexity

For:

- `n` points;
- `k` clusters;
- `d` features;
- `i` iterations;

the main cost is approximately:

```text
O(i × n × k × d)
```

Most of the work occurs during assignment because every point is compared with every centroid during every iteration.

## PCA complexity

For covariance-based PCA:

```text
Centring:            O(n × d)
Covariance:          O(n × d²)
Eigendecomposition:  O(d³)
```

For RGB data, `d = 3`, so eigendecomposition is inexpensive. The number of pixels still affects centring, covariance calculation, and visualization.

## Sampling

The Streamlit application fits K-means and PCA on reproducibly sampled pixels rather than every pixel of a large image.

For compression, the learned centroids are then applied to the full pixel matrix.

Sampling reduces interactive runtime and memory use while still learning representative colours from the image.

## Streamlit laboratory

The application contains four tabs:

### Compress Image

The user can select or upload an image, choose `k`, select random or K-means++ initialization, and inspect:

- original and compressed images;
- learned palette;
- colour counts;
- RGB MSE;
- iterations;
- runtime.

### Explore Similar Pixels

The user selects a query colour, Euclidean or Manhattan distance, and the number of neighbours. The application returns nearby RGB colours and their distances.

### PCA & Clusters

The application shows:

- explained variance;
- a PC1-versus-PC2 projection;
- K-means assignments in PCA space.

### Experiments

The application presents saved results for:

- different values of `k`;
- initialization sensitivity;
- feature scaling;
- scikit-learn runtime comparisons;
- curse of dimensionality.

The scaling section is a conceptual explanation rather than a standalone saved numerical experiment.

## Engineering support

The repository also includes:

- reusable source modules;
- input validation;
- automated tests;
- YAML configuration;
- GitHub Actions CI;
- experiment scripts;
- saved reports and figures;
- complexity documentation;
- Docker support;
- a deployed Streamlit application.

## Limitations

- RGB MSE is not a complete perceptual-quality metric.
- Experiments use one main sample image.
- Brute-force distances can require substantial temporary memory.
- Small runtime benchmarks should not be generalized.
- The laboratory does not use perceptual colour spaces or learned image embeddings.
- PCA reduces only three RGB dimensions in the main workflow.
- The application samples pixels to keep interactive calculations manageable.

## Key takeaway

Professional comparison is not only about obtaining the same output. It requires understanding valid differences, controlling experimental conditions, measuring runtime carefully, and knowing when a learning implementation should be replaced by an optimized library.

## Related notes

- [Distance Metrics, Broadcasting, and KNN](Distance%20Metrics,%20Broadcasting,%20and%20KNN.md)
- [Compression Evaluation and k Trade-offs](Compression%20Evaluation%20and%20k%20Trade-offs.md)
- [PCA from Scratch and Explained Variance](PCA%20from%20Scratch%20and%20Explained%20Variance.md)