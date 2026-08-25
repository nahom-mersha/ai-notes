# Classification Metrics, Thresholds, and Class Imbalance

## Confusion matrix

A confusion matrix separates predictions into four outcomes:

| Outcome | Meaning |
|---|---|
| True positive | Predicted churn and the customer churned |
| False positive | Predicted churn but the customer stayed |
| False negative | Predicted non-churn but the customer churned |
| True negative | Predicted non-churn and the customer stayed |

For a retention workflow:

- false positives create unnecessary contact or offer costs;
- false negatives represent missed churners.

## Precision

Precision asks:

```text
Of the customers predicted to churn,
how many actually churned?
```

```text
precision = TP / (TP + FP)
```

High precision means the flagged group contains relatively few false alarms.

## Recall

Recall asks:

```text
Of all customers who actually churned,
how many did the model catch?
```

```text
recall = TP / (TP + FN)
```

High recall means few real churners were missed.

## F1 score

F1 combines precision and recall through their harmonic mean:

```text
F1 = 2 × precision × recall / (precision + recall)
```

It is useful when both matter, but it still assumes a particular balance between them. A business cost model may require a different trade-off.

## Why accuracy is insufficient

The majority baseline achieved accuracy of `0.7346` while detecting zero churners.

This happened because non-churners were the majority. Accuracy counted the many correctly predicted non-churners but did not communicate that every positive case was missed.

## Thresholds change decisions

A classification model produces probabilities. The threshold determines which probabilities become positive labels.

The NumPy logistic-regression experiment produced:

| Threshold | Precision | Recall | F1 |
|---|---:|---:|---:|
| 0.30 | 0.5246 | 0.7406 | 0.6142 |
| 0.50 | 0.6401 | 0.5374 | 0.5843 |
| 0.70 | 0.7292 | 0.1872 | 0.2979 |

Lowering the threshold:

- predicts churn for more customers;
- catches more churners;
- increases recall;
- usually creates more false positives;
- usually reduces precision.

Raising the threshold produces the opposite behavior.

Although `0.30` had the highest F1 among these three diagnostic thresholds, it was not selected as the final threshold. Doing so would use the held-out test set to make a development decision.

## What I learned

I learned that a model does not possess one fixed precision and recall independently of its threshold.

The model produces probability scores. The operating policy determines which errors are accepted.

## Key takeaway

Metric selection and threshold selection must follow the real decision problem. Accuracy and a default threshold of `0.50` are not automatically appropriate.

## Related notes

- [Classification Problem, Data, and Baseline](Classification%20Problem%2C%20Data%2C%20and%20Baseline.md)
- [Ranking Quality, Calibration, and Error Analysis](Ranking%20Quality%2C%20Calibration%2C%20and%20Error%20Analysis.md)
- [Business-Cost-Aware Threshold Selection](Business-Cost-Aware%20Threshold%20Selection.md)