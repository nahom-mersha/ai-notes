# Business-Cost-Aware Threshold Selection

## Why a business threshold is needed

The model produces churn probabilities, but a retention workflow needs a decision:

```text
contact customer
or
do not contact customer
```

The decision rule is:

```python
if churn_probability >= threshold:
    contact_customer = True
```

A threshold of `0.50` is only a default convention. It is not automatically the best business policy.

## Illustrative cost model

The project uses the following assumptions:

| Outcome | Net value |
|---|---:|
| True positive | +€180 |
| False positive | -€20 |
| False negative | -€180 |
| True negative | €0 |

Interpretation:

- contacting a real churner produces an assumed €200 retention value but costs €20, giving a net value of €180;
- contacting a non-churner costs €20;
- missing a churner loses an assumed €180 opportunity;
- leaving a non-churner alone has no assigned value.

The formula is:

```text
net value =
    180 × TP
    - 20 × FP
    - 180 × FN
```

These values are illustrative, not universal business facts.

## Leakage-safe threshold selection

Candidate thresholds from `0.10` to `0.90` were evaluated using out-of-fold training probabilities.

The held-out test set was not used to select the threshold.

## Results

For Gradient Boosting, the best out-of-fold result was:

```text
threshold = 0.10
TP = 1420
FP = 2146
FN = 75
TN = 1993
net value = €199,180
```

At the default threshold:

```text
threshold = 0.50
TP = 789
FP = 384
FN = 706
TN = 3755
net value = €7,260
```

The lower threshold creates many more false positives, but it reduces false negatives dramatically.

Gradient Boosting at `0.10` also slightly exceeded Logistic Regression’s best out-of-fold net value:

```text
Gradient Boosting = €199,180
Logistic Regression = €195,580
difference = €3,600
```

## Final decision

The selected policy is:

```text
model = Gradient Boosting
threshold = 0.10
```

Final held-out test results:

| Metric | Value |
|---|---:|
| TN | 492 |
| FP | 543 |
| FN | 17 |
| TP | 357 |
| Precision | 0.3967 |
| Recall | 0.9545 |
| F1 | 0.5604 |
| ROC-AUC | 0.8467 |
| Average Precision | 0.6684 |
| Brier score | 0.1349 |
| Net value | €50,340 |

The threshold was not changed after this evaluation.

## Interpretation

The policy is intentionally aggressive:

- most real churners are caught;
- few churners are missed;
- many customers who would stay are also contacted.

This trade-off makes sense only because the assumed false-negative cost is much larger than the false-positive cost.

If the costs change, the optimal threshold can also change.

## Key takeaway

A classification threshold is a decision policy built on assumptions about consequences. It is not an intrinsic property of the trained model.

## Related notes

- [Classification Metrics, Thresholds, and Class Imbalance](Classification%20Metrics%2C%20Thresholds%2C%20and%20Class%20Imbalance.md)
- [Ranking Quality, Calibration, and Error Analysis](Ranking%20Quality%2C%20Calibration%2C%20and%20Error%20Analysis.md)
- [Reproducible Training and Model Artifacts](Reproducible%20Training%20and%20Model%20Artifacts.md)