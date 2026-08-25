# Batch Prediction, FastAPI, Testing, and Logging

## Shared inference contract

Both inference workflows use the saved preprocessing-and-model pipeline and metadata.

```text
configuration
-> model artifact
-> metadata
-> validated customer features
-> churn probability
-> threshold
-> predicted label
```

Neither workflow retrains the model during prediction.

## Batch prediction

The batch command is:

```bash
python scripts/predict_batch.py \
  --input data/processed/telco_churn_clean.csv \
  --output reports/batch_predictions.csv
```

The batch workflow:

1. loads configuration;
2. loads model metadata;
3. loads the saved pipeline;
4. reads the input CSV;
5. checks for required feature columns;
6. produces churn probabilities;
7. applies the saved threshold;
8. writes the output CSV.

The output contains the original data plus:

```text
churn_probability
predicted_churn
decision_threshold
```

Including the threshold makes the decision policy visible in the output.

## FastAPI

The API can be started with:

```bash
uvicorn customer_churn_classifier.api:app --reload
```

The project exposes:

```text
GET  /
GET  /health
POST /predict
```

The `/predict` endpoint accepts one customer record and returns:

```json
{
  "churn_probability": 0.6672,
  "decision_threshold": 0.10,
  "predicted_churn": 1
}
```

Pydantic validates required fields and rejects unexpected fields.

The application factory accepts optional model and metadata objects. This dependency-injection pattern allows API behavior to be tested without loading the real model artifact.

## Testing

The test suite includes checks for:

- analytical gradients versus numerical gradients;
- the API health endpoint;
- valid API prediction responses;
- missing required API fields;
- batch output columns;
- probability-to-label conversion;
- saved threshold values;
- reusable statistical helpers.

Tests use fake models where appropriate. This isolates interface behavior from the cost of real model training.

`tmp_path` provides temporary files and directories for batch tests. `monkeypatch` temporarily replaces model loading with a controlled fake model.

## Continuous integration

GitHub Actions runs on pushes and pull requests.

The workflow:

```text
installs the project
-> checks formatting
-> runs Ruff linting
-> runs pytest
```

This provides an automated check that the committed project remains internally consistent.

## Logging

Reusable modules create named loggers:

```python
logger = logging.getLogger(__name__)
```

Runnable entry points configure how log messages appear.

This allows multiple modules to report events through one logging configuration for each running process.

The project logs events such as:

- configuration loading;
- dataset loading;
- model training;
- artifact saving;
- input validation;
- completed predictions;
- prediction failures.

Customer feature values are not written into routine log messages.

## Limitations

The API is a local learning implementation, not a hardened production service. It does not include authentication, rate limiting, deployment monitoring, drift detection, or a production retraining policy.

## Key takeaway

A usable ML system needs reliable inference paths, validated inputs, reproducible artifacts, tests, logging, and clear operational limitations—not only a trained classifier.

## Related notes

- [Reproducible Training and Model Artifacts](Reproducible%20Training%20and%20Model%20Artifacts.md)
- [Business-Cost-Aware Threshold Selection](Business-Cost-Aware%20Threshold%20Selection.md)