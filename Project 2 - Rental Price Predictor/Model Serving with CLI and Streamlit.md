# Model Serving with CLI and Streamlit

## Overview

Training a model is not the same as making it usable. The project saves the fitted pipeline, validates new inputs, and exposes the same prediction logic through a command-line interface and a public Streamlit application.

## Model serialization

The selected enhanced Ridge pipeline is saved as:

```text
models/enhanced_ridge.joblib
```

This artifact contains the fitted pipeline:

- learned numerical imputation values;
- learned scaling statistics;
- learned categorical encoding;
- Ridge coefficients and intercept.

It does not contain the complete raw rental dataset.

Saving the full pipeline is important because new inputs must receive exactly the same transformations used during training.

## Shared prediction function

The reusable `predict_rent()` function:

- validates required numerical and text inputs;
- rejects invalid ranges and non-Boolean amenities;
- converts optional missing values into a representation handled by the fitted imputers;
- creates a one-row DataFrame with the expected feature names;
- returns the predicted monthly cold rent as a float.

The interfaces call this shared function rather than duplicating prediction logic.

```text
saved model
→ predict_rent()
→ input validation
↙                 ↘
CLI             Streamlit
```

## Command-line interface

The CLI uses `argparse` and accepts required values such as living space, number of rooms, and neighbourhood.

Boolean flags use `store_true`:

```bash
python -m rental_price_predictor.cli \
  --living-space 70 \
  --no-rooms 2 \
  --neighbourhood Schwabing \
  --lift \
  --has-kitchen \
  --cellar
```

The command prints the estimated monthly cold rent and reports understandable errors for invalid inputs.

## Streamlit application

The web application provides form controls for apartment details and amenities. It loads the saved model with:

```python
@st.cache_resource
```

Streamlit reruns the script when a user changes an input. Caching allows the already-loaded model to be reused instead of reading the artifact from disk on every rerun.

The public application makes the project easier to demonstrate because a visitor can try it without cloning the repository or installing Python.

## Responsible use

The model card explains that the predictor is an educational estimate, not a professional valuation.

Important limitations include:

- historical Munich listing data only;
- weaker reliability for luxury or unusual apartments;
- possible reporting errors and biases in the source listings;
- statistical associations rather than causal relationships;
- amenity checkboxes represent known presence or absence, not an unknown state.

The enhanced model achieved a held-out MAE of €289.13, but this average does not guarantee that every individual prediction is close.

## Key takeaway

A deployable ML system needs a saved preprocessing-and-model pipeline, reusable prediction logic, validated inputs, user-facing interfaces, and clear limits on how its output should be used.

## Related notes

- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)
- [Error Analysis and Feature Effects](Error%20Analysis%20and%20Feature%20Effects.md)