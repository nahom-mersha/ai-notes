# Data Quality Checks and Validation

## Overview

A data-quality check identifies problems that could affect analysis or machine-learning results.

The inspector checks common problems at both dataset and column level.

## Core checks

### Missing values

Missing values mean that information is unavailable.

```text
age
22
<missing>
31
```

Many ML models cannot use missing values directly. Depending on the situation, missing values may be removed, filled, or separately modelled.

### Duplicate rows

Duplicate records can give some examples extra influence. They can also make model evaluation overly optimistic if similar records appear in both training and test data.

### Constant columns

A constant column has the same value in every row.

```text
country
Germany
Germany
Germany
```

It usually provides no useful information for distinguishing predictions.

## Validation schema

Descriptive checks show what exists in the data. Validation compares values with what they are expected to mean.

For example:

```yaml
columns:
  age:
    type: integer
    minimum: 0
    maximum: 120
```

This can flag non-numeric ages, negative ages, and values above the allowed maximum.

## Key takeaway

A useful data inspector does more than summarize a dataset: it compares values against explicit expectations.

## Related notes

- [Data Quality Warnings for ML](Data%20Quality%20Warnings%20for%20ML.md)
- [Data Quality Reports](Data%20Quality%20Reports.md)