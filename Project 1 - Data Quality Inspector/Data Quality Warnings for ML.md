# Data Quality Warnings for ML

## Overview

Some data problems are warnings rather than automatic errors. They need context before deciding whether to remove, transform, or keep the data.

## Outliers

An outlier is a value far from most other values.

A Z-score can flag unusual numeric values. However, an outlier may be valid. For example, a very expensive house may be unusual but real.

## Class imbalance

Class imbalance happens when one target class is much more common than another.

```text
bought_product
no:  950
yes: 50
```

A model that always predicts `no` could appear accurate while failing to identify the less common class.

## Correlation

Correlation measures how numeric variables move together.

A strong correlation can reveal useful relationships, duplicated information, or features that need further investigation.

Correlation does not prove causation.

## Possible target leakage

Target leakage happens when a feature contains information that would not be available when making a real prediction.

Examples:

- predicting loan repayment while using `loan_repaid`;
- predicting sale price while using information known only after the sale.

An automated tool can flag suspicious signals, but it cannot prove leakage without domain knowledge.

## Categorical consistency

These values may represent the same category:

```text
Berlin
berlin
BERLIN
```

A model may treat them as separate categories and create unnecessary features. Values should be normalized only after confirming that they truly mean the same thing.

## Key takeaway

A data-quality tool should flag risks clearly without treating every warning as a confirmed error.

## Related notes

- [Data Quality Checks and Validation](Data%20Quality%20Checks%20and%20Validation.md)
- [Scaling and Train-Test Splitting](Scaling%20and%20Train-Test%20Splitting.md)