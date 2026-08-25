# Leakage-Safe Pipelines and Professional Model Comparison

## Train-test structure

The project uses a fixed stratified train-test split:

```text
Full dataset
├── Training set
│   └── Cross-validation and development decisions
└── Held-out test set
    └── Final evaluation
```

Stratification keeps the churn proportion similar across the two sets.

The held-out test set must not influence model selection, calibration decisions, or threshold selection. Otherwise, its final score becomes an optimistic estimate rather than an independent evaluation.

## Leakage-safe preprocessing

The project uses a `ColumnTransformer` inside scikit-learn pipelines.

Numerical features receive:

- median imputation;
- standard scaling.

Categorical features receive:

- missing-value handling;
- one-hot encoding;
- unknown-category handling during inference.

Because preprocessing is inside the pipeline, each cross-validation fold fits its imputer, scaler, and encoder using only that fold’s training portion.

The flow is:

```text
training fold
-> fit preprocessing
-> transform training fold
-> fit classifier
-> transform validation fold
-> evaluate classifier
```

This prevents validation information from leaking into learned preprocessing values.

## Models compared

The professional comparison included:

- Logistic Regression;
- K-Nearest Neighbours;
- Decision Tree;
- Random Forest;
- Gradient Boosting.

All models used the same five-fold stratified cross-validation procedure and evaluation metrics.

## Baseline comparison

| Model | ROC-AUC | Average Precision |
|---|---:|---:|
| Logistic Regression | 0.8461 ± 0.0125 | 0.6614 ± 0.0195 |
| KNN | 0.7827 ± 0.0066 | 0.5073 ± 0.0167 |
| Decision Tree | 0.6583 ± 0.0124 | 0.3804 ± 0.0130 |
| Random Forest | 0.8182 ± 0.0123 | 0.6107 ± 0.0296 |
| Gradient Boosting | 0.8478 ± 0.0122 | 0.6671 ± 0.0232 |

The single Decision Tree performed worst. Random Forest improved on it by averaging many trees.

Logistic Regression and Gradient Boosting were the strongest candidates.

## Hyperparameter tuning

Grid search retained the default Logistic Regression setting:

```text
C = 1.0
Mean CV Average Precision = 0.6614
```

The best Gradient Boosting settings were:

```text
learning_rate = 0.1
max_depth = 1
n_estimators = 200
Mean CV Average Precision = 0.6705
```

Many shallow trees worked better than the other tested configurations.

## Final model-selection reasoning

Gradient Boosting moved forward because it achieved the highest cross-validated Average Precision and ROC-AUC.

The performance difference from Logistic Regression was small relative to cross-validation variation. Therefore, the result should be treated as a reasoned selection rather than proof that Gradient Boosting is universally superior.

Logistic Regression remained useful because it was:

- nearly as accurate in ranking terms;
- slightly stronger in recall at the default threshold;
- simpler;
- easier to interpret.

## Key takeaway

Fair model comparison requires consistent data splits, leakage-safe preprocessing, cross-validation, predefined metrics, and honest treatment of small performance differences.

## Related notes

- [Classification Problem, Data, and Baseline](Classification%20Problem%2C%20Data%2C%20and%20Baseline.md)
- [Ranking Quality, Calibration, and Error Analysis](Ranking%20Quality%2C%20Calibration%2C%20and%20Error%20Analysis.md)