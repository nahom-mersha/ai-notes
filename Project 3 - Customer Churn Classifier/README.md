# Customer Churn Classifier

Notes from Project 3 of my AI engineering roadmap.

This project includes an end-to-end classification workflow that estimates whether a telecom customer is likely to churn. It combines a learning-focused NumPy implementation of logistic regression with professional scikit-learn model comparison, probability evaluation, cost-aware threshold selection, reproducible training, batch prediction, and a FastAPI endpoint.

- Project repository: https://github.com/nahom-mersha/customer-churn-classifier

## AI assistance

This is an AI-assisted learning project. I used ChatGPT to help generate and explain code, then reviewed the implementation, ran tests, explored the underlying concepts, and documented what I learned.

## What the project covers

- Binary classification and churn-probability prediction
- Dataset documentation, cleaning, validation, and class imbalance
- Majority-class baseline evaluation
- Logistic regression from scratch with NumPy
- Sigmoid, binary cross-entropy, vectorized gradients, and gradient descent
- Numerical gradient checking
- Learning-rate and feature-scaling experiments
- Leakage-safe preprocessing with scikit-learn pipelines
- Cross-validation and professional model comparison
- Precision, recall, F1, ROC-AUC, Average Precision, and confusion matrices
- Ranking quality, calibration, Brier score, and error analysis
- Business-cost-aware threshold selection
- Configuration-driven training and saved model metadata
- Batch prediction, FastAPI, testing, logging, and CI
- Model limitations and responsible use

## Final model

The selected model is Gradient Boosting with:

```text
learning_rate = 0.1
max_depth = 1
n_estimators = 200
```

The selected business-cost-aware threshold is:

```text
0.10
```

This deliberately low threshold prioritizes catching churners because the illustrative cost model treats a missed churner as much more expensive than an unnecessary retention contact.

Final held-out test results:

| Metric | Value |
|---|---:|
| Precision | 0.3967 |
| Recall | 0.9545 |
| F1 | 0.5604 |
| ROC-AUC | 0.8467 |
| Average Precision | 0.6684 |
| Brier score | 0.1349 |
| Net value | €50,340 |

These results belong to the project’s illustrative assumptions and public learning dataset. They are not evidence of production performance for a real telecom company.

## Notes

- [Classification Problem, Data, and Baseline](Classification%20Problem%2C%20Data%2C%20and%20Baseline.md)
- [Logistic Regression from Scratch with NumPy](Logistic%20Regression%20from%20Scratch%20with%20NumPy.md)
- [Gradient Checking, Learning Rates, and Scaling](Gradient%20Checking%2C%20Learning%20Rates%2C%20and%20Scaling.md)
- [Classification Metrics, Thresholds, and Class Imbalance](Classification%20Metrics%2C%20Thresholds%2C%20and%20Class%20Imbalance.md)
- [Leakage-Safe Pipelines and Professional Model Comparison](Leakage-Safe%20Pipelines%20and%20Professional%20Model%20Comparison.md)
- [Ranking Quality, Calibration, and Error Analysis](Ranking%20Quality%2C%20Calibration%2C%20and%20Error%20Analysis.md)
- [Business-Cost-Aware Threshold Selection](Business-Cost-Aware%20Threshold%20Selection.md)
- [Reproducible Training and Model Artifacts](Reproducible%20Training%20and%20Model%20Artifacts.md)
- [Batch Prediction, FastAPI, Testing, and Logging](Batch%20Prediction%2C%20FastAPI%2C%20Testing%2C%20and%20Logging.md)

## Key takeaway

A classification system is more than a model that produces labels. The data split, probability quality, decision threshold, business costs, inference workflow, tests, and limitations all affect whether its predictions are useful.