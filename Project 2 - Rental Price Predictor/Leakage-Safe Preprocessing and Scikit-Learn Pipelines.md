# Leakage-Safe Preprocessing and Scikit-Learn Pipelines

## Overview

Preprocessing is part of the model workflow. Imputation values, scaling statistics, and category mappings are learned from data, so they must not be fitted using validation or test rows.

## Correct evaluation order

The safe flow is:

```text
clean the data
→ split into training and test data
→ fit preprocessing on training data only
→ apply it to training and test data
→ fit the model on the training data
→ evaluate on the test data
```

For the NumPy model, the feature means and standard deviations come only from `X_train`. The same values are then applied to `X_test`.

The scaled test data does not need an exact mean of zero or standard deviation of one. It is unseen data drawn from a separate sample.

## ColumnTransformer

The professional models contain different types of inputs, so the project uses a `ColumnTransformer` to process them separately.

### Numerical pipeline

```text
numerical features
→ median imputation
→ standardization
```

Median imputation is less sensitive to extreme rental values than mean imputation. Standardization is especially important for Ridge because its penalty acts on coefficient sizes.

### Categorical pipeline

```text
categorical features
→ replace missing values with "missing"
→ one-hot encoding
```

A separate missing category preserves the fact that the information was unavailable.

The encoder uses:

- `drop="first"` to remove one redundant indicator from each categorical feature;
- `handle_unknown="ignore"` so a previously unseen category does not crash prediction.

## Complete pipeline

The preprocessing block and estimator are joined together:

```text
raw feature row
→ ColumnTransformer
→ combined numerical and encoded features
→ regression model
→ predicted rent
```

This structure means `.fit()` learns the preprocessing values and model parameters together. `.predict()` then applies the same fitted transformations to new rows.

## Cross-validation safety

Keeping preprocessing inside the pipeline also prevents leakage during cross-validation.

For each fold, the imputer, scaler, and encoder are fitted only on that fold's training portion and then applied to its validation portion. They are not fitted once on all folds before evaluation.

## Key takeaway

A model is not only its final estimator. It is the complete, repeatable chain that turns raw inputs into predictions without learning from evaluation data.

## Related notes

- [Rental Prediction Problem and Data Preparation](Rental%20Prediction%20Problem%20and%20Data%20Preparation.md)
- [Cross-Validation, Regularization, and Model Selection](Cross-Validation%2C%20Regularization%2C%20and%20Model%20Selection.md)
- [Model Serving with CLI and Streamlit](Model%20Serving%20with%20CLI%20and%20Streamlit.md)