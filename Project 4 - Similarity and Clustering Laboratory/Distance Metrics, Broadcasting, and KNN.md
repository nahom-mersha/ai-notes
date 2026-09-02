# Distance Metrics, Broadcasting, and KNN

## Overview

Distance-based algorithms need a numerical definition of similarity.

In this project, RGB colours are treated as vectors:

```text
colour = [red, green, blue]
```

Two colours are considered similar when the selected distance between their vectors is small.

## Euclidean distance

Euclidean distance measures straight-line distance.

For two RGB colours:

```text
first  = [R1, G1, B1]
second = [R2, G2, B2]
```

the calculation is:

```text
sqrt(
    (R1 - R2)²
  + (G1 - G2)²
  + (B1 - B2)²
)
```

Euclidean distance gives larger differences extra influence because the channel differences are squared.

## Manhattan distance

Manhattan distance adds the absolute difference in every feature:

```text
|R1 - R2| + |G1 - G2| + |B1 - B2|
```

For:

```text
first  = [100, 120, 200]
second = [110, 100, 190]
```

the Manhattan distance is:

```text
10 + 20 + 10 = 40
```

Euclidean and Manhattan distance can rank neighbours differently because they combine feature differences differently.

## Pairwise distance calculations

A KNN query usually needs distances between many queries and many references.

Suppose:

```text
queries.shape    = (Q, D)
references.shape = (R, D)
```

where:

- `Q` is the number of query points;
- `R` is the number of reference points;
- `D` is the number of features.

The expected distance matrix has shape:

```text
(Q, R)
```

Each value means:

```text
distances[query_index, reference_index]
```

## NumPy broadcasting

The pairwise implementation creates compatible shapes:

```text
queries[:, np.newaxis, :]    → (Q, 1, D)
references[np.newaxis, :, :] → (1, R, D)
```

NumPy broadcasts the subtraction to:

```text
(Q, R, D)
```

This contains the feature differences for every query-reference pair.

For Euclidean distance, the project then:

1. Squares every difference.
2. Sums across the feature axis.
3. Takes the square root.

The reduction across the final axis changes:

```text
(Q, R, D)
```

into:

```text
(Q, R)
```

The placement of `np.newaxis` is determined by the desired meaning of the output dimensions. It is not an arbitrary formatting choice.

## Brute-force KNN

The project includes a brute-force K-nearest-neighbour search.

For each query, it:

1. Calculates the distance to every reference.
2. Sorts the reference indices by distance.
3. Keeps the first `k` indices.
4. Retrieves the corresponding distances.

Conceptually:

```text
query
  ↓
distance to every reference
  ↓
sort from nearest to farthest
  ↓
keep first k
```

It is called brute force because it checks every reference instead of using a specialized search index.

## `k` has different meanings

K-means and KNN both use the letter `k`, but it means different things.

| Algorithm | Meaning of `k` |
| --- | --- |
| K-means | Number of clusters |
| KNN | Number of neighbours returned |

K-means learns representative group centres. KNN does not learn centroids; it retrieves existing nearby points for a query.

## KNN in the laboratory

The Streamlit application lets the user:

- choose a query colour;
- select Euclidean or Manhattan distance;
- select the number of neighbours;
- view the nearest colours that actually occur in the selected image;
- inspect their RGB values and distances.

## Limitations

Brute-force KNN is easy to understand and verify, but it becomes expensive as the number of queries, reference points, or dimensions grows.

Equal-distance points can also produce different but equally valid neighbour orders. This matters when comparing exact neighbour indices with another implementation.

## Key takeaway

Vectorization removes explicit Python loops, but it does not remove the underlying work. Broadcasting still constructs and processes the query-reference relationships that the algorithm needs.

## Related notes

- [Image Data and Colour Quantization](Image%20Data%20and%20Colour%20Quantization.md)
- [Feature Scaling and the Curse of Dimensionality](Feature%20Scaling%20and%20the%20Curse%20of%20Dimensionality.md)
- [Scikit-Learn Comparison, Complexity, and Interactive Delivery](Scikit-Learn%20Comparison,%20Complexity,%20and%20Interactive%20Delivery.md)