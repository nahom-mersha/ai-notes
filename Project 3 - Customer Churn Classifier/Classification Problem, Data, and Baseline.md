# Classification Problem, Data, and Baseline

## Problem definition

The project treats customer churn as a binary classification problem.

Each row represents one telecom customer. The model receives information that could reasonably be available before a retention decision, including:

- tenure;
- contract type;
- monthly and total charges;
- payment method;
- subscribed services;
- selected account characteristics.

The output is a probability:

```text
P(customer churns | available customer features)
```

A separate decision threshold converts this probability into a label:

```text
0 = predicted non-churn
1 = predicted churn
```

Keeping probability prediction separate from the final decision is important because different business situations can require different thresholds.

## Dataset

The project uses the IBM Telco Customer Churn dataset.

It contains:

```text
7,043 customers
21 original columns
```

The target originally contains `Yes` and `No` values and is converted to:

```text
Yes = 1
No  = 0
```

The `customerID` column is excluded because it is an identifier rather than a meaningful predictor.

`TotalCharges` requires special handling because 11 values are stored as blank strings. These values are converted to missing numerical values before preprocessing.

The dataset is fictional and does not provide a well-defined future prediction window. It is useful for learning classification, but it does not prove that the resulting model would perform well for a real company.

## Class imbalance

Churn is the minority class. Most customers did not churn.

This makes accuracy potentially misleading. A model can achieve apparently respectable accuracy by predicting that every customer will stay, while catching no churners.

## Majority-class baseline

The baseline always predicts the most common training class:

```text
predicted class = non-churn
```

Its held-out results were:

| Metric | Value |
|---|---:|
| Accuracy | 0.7346 |
| Churn precision | 0.0000 |
| Churn recall | 0.0000 |
| Churn F1 | 0.0000 |

Confusion matrix:

```text
TN = 1035
FP = 0
FN = 374
TP = 0
```

The accuracy looks acceptable only because non-churners are the majority. The baseline misses every churner and is therefore useless for a retention workflow.

## What I learned

I learned that a good classification problem must define:

- what one row represents;
- when the prediction is made;
- what the positive class means;
- which information is available at prediction time;
- who uses the output;
- what action follows from the prediction;
- which mistakes matter most.

## Key takeaway

A baseline provides the minimum standard a learned model must beat. For imbalanced classification, beating baseline accuracy is not enough—the model must actually identify useful positive cases.

## Related notes

- [Classification Metrics, Thresholds, and Class Imbalance](Classification%20Metrics%2C%20Thresholds%2C%20and%20Class%20Imbalance.md)
- [Leakage-Safe Pipelines and Professional Model Comparison](Leakage-Safe%20Pipelines%20and%20Professional%20Model%20Comparison.md)