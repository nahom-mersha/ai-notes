# Scaling and Train-Test Splitting

## Overview

This project includes min-max scaling, standardization, and train-test splitting utilities.

Some ML workflows need numerical features on comparable scales.

## Min-max scaling

Min-max scaling maps values to a chosen range, commonly 0 to 1.

```text
scaled_value = (value - minimum) / (maximum - minimum)
```

## Standardization

Standardization transforms values so they have a mean of 0 and a standard deviation of 1.

```text
z = (value - mean) / standard_deviation
```

## Why scaling matters

Consider:

```text
age:     18 to 70
income:  20,000 to 200,000
```

For models that use distances or gradient-based optimization, larger numeric ranges can dominate smaller ones.

Scaling is not equally important for every model. Tree-based models are usually less sensitive to feature scale.

## Train-test split

A train-test split separates data used to learn patterns from data used to evaluate the model.

The test set should represent data that the model did not see during training.

## Common mistake

Do not calculate scaling values using the full dataset before splitting. Learn the minimum, maximum, mean, and standard deviation from the training data, then apply them to the test data.

Otherwise, information from the test set leaks into training.

## Key takeaway

Preprocessing is part of a valid evaluation pipeline, not only a way to transform numbers.

## Related notes

- [Manual Statistics and NumPy](Manual%20Statistics%20and%20NumPy.md)
- [Data Quality Warnings for ML](Data%20Quality%20Warnings%20for%20ML.md)