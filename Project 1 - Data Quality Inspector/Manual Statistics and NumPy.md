# Manual Statistics and NumPy

## Overview

This project includes manual and NumPy-based descriptive-statistics utilities.

I learned the calculations behind common data-science tools by reviewing both approaches.

## Main statistics

- **Mean:** average value
- **Median:** middle value after sorting
- **Variance:** how spread out values are
- **Standard deviation:** typical distance from the mean
- **Quantiles:** cut points such as 25%, 50%, and 75%
- **Z-score:** how unusual a value is relative to the mean

## Example

For:

```text
[2, 4, 6, 8]
```

The mean is:

```text
(2 + 4 + 6 + 8) / 4 = 5
```

## Manual vs. NumPy

Manual calculations are useful for learning and testing the logic.

NumPy is better for practical numerical work because it efficiently operates on arrays and is part of the foundation of the Python data stack.

## Common mistake

A statistic can be mathematically correct but still misleading. For example, extreme values can strongly affect the mean.

## Key takeaway

Reviewing manual calculations helped me use NumPy as a tool I understand rather than a black box.

## Related notes

- [Scaling and Train-Test Splitting](Scaling%20and%20Train-Test%20Splitting.md)
- [Data Quality Warnings for ML](Data%20Quality%20Warnings%20for%20ML.md)