# Reproducible Training and Model Artifacts

## Configuration-driven training

The final model settings are stored in:

```text
configs/final_model.yaml
```

The configuration records:

- model name;
- selected threshold;
- Gradient Boosting hyperparameters;
- random seed;
- model and metadata paths;
- overwrite behavior.

This separates experiment decisions from the training logic.

The final training command is:

```bash
python scripts/train_final_model.py
```

## Training workflow

The command:

1. loads the configuration;
2. loads the cleaned dataset;
3. creates the reproducible train-test split;
4. builds the preprocessing-and-model pipeline;
5. applies the selected hyperparameters;
6. fits the pipeline;
7. evaluates the fixed threshold;
8. saves the trained pipeline;
9. writes reproducibility metadata.

## Saved pipeline

The generated model artifact is:

```text
models/customer_churn_gradient_boosting.joblib
```

It contains both:

```text
fitted preprocessing
+
fitted Gradient Boosting classifier
```

Keeping them together is essential. New customer records must receive the same imputation, scaling, category encoding, and feature ordering used during training.

Saving only the classifier would require reconstructing preprocessing separately and could cause training-serving inconsistencies.

The binary artifact is generated locally and ignored by Git because it is a reproducible generated output.

## Metadata

The committed metadata file is:

```text
models/customer_churn_gradient_boosting_metadata.json
```

It records:

- creation timestamp;
- model name;
- hyperparameters;
- threshold;
- random seed;
- train and test sample counts;
- required input features;
- confusion-matrix counts;
- final evaluation metrics;
- artifact paths.

This makes the saved model easier to inspect without loading the binary pipeline.

## Overwrite policy

The configuration includes:

```yaml
overwrite: true
```

The training script checks this setting before replacing existing model or metadata files.

A larger production system would normally use immutable, versioned artifacts rather than repeatedly overwriting one path. The simpler policy is suitable for this learning project as long as it is documented.

## Reproducibility limits

A fixed random seed and stored configuration improve reproducibility, but they do not guarantee perfect reproducibility across every machine.

Results can still depend on:

- library versions;
- platform differences;
- changes to the input dataset;
- changes to preprocessing or training code.

## What I learned

I learned that model persistence involves more than saving learned coefficients. A usable artifact needs its preprocessing, threshold, feature contract, configuration, metadata, and regeneration command.

## Key takeaway

A reproducible ML artifact connects training decisions with inference behavior. The saved pipeline and metadata form the contract used by downstream interfaces.

## Related notes

- [Leakage-Safe Pipelines and Professional Model Comparison](Leakage-Safe%20Pipelines%20and%20Professional%20Model%20Comparison.md)
- [Business-Cost-Aware Threshold Selection](Business-Cost-Aware%20Threshold%20Selection.md)
- [Batch Prediction, FastAPI, Testing, and Logging](Batch%20Prediction%2C%20FastAPI%2C%20Testing%2C%20and%20Logging.md)