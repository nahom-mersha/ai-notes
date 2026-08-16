# Rental Prediction Problem and Data Preparation

## Overview

This project treats rent prediction as a supervised regression problem.

Each row represents one advertised rental property. The model receives property information and predicts:

```text
baseRent = monthly cold rent in euros
```

Cold rent was chosen because it represents the advertised base price before service charges and heating costs. It is clearer than mixing several housing-cost components into one target.

## Dataset scope

The project uses the public Kaggle dataset **Apartment Rental Offers in Germany**, collected from ImmoScout24 listings.

| Stage | Listings |
| --- | ---: |
| Full German dataset | 268,850 |
| Munich city subset | 4,383 |
| Final cleaned Munich data | 4,380 |

The project focuses on Munich because rental markets differ greatly between German cities. This makes neighbourhood information more meaningful, but it also means the model should not be presented as a Germany-wide predictor.

## Feature choices

The learning-focused NumPy model begins with two numerical features:

- `livingSpace`
- `noRooms`

This small feature set makes the prediction equation, loss, gradients, and optimization easier to inspect.

The enhanced professional model also uses neighbourhood, flat type, construction year, floor, condition, interior quality, and amenities such as a balcony, lift, kitchen, garden, and cellar.

## Leakage exclusions

Fields such as `totalRent`, `serviceCharge`, and `heatingCosts` were excluded from the model inputs.

They are too directly connected to `baseRent`:

```text
total rent ≈ base rent + service charge + heating costs
```

Using them would make the prediction task unrealistic and could leak information that nearly reveals the target.

## Cleaning decisions

The project applies narrow validity rules:

- keep `baseRent >= 100`;
- keep `livingSpace <= 500`;
- exclude one validated target error at index `213625`.

That listing recorded a cold rent of €20,100, while its total rent and additional charges implied approximately €2,100. The row was excluded rather than silently corrected because the intended value was not authoritative.

Other expensive listings were kept when their rent fields were internally consistent. An unusual value is not automatically an error.

## Limitations

- The data contains advertised listings rather than signed contracts.
- It represents a historical collection period rather than the current market.
- Some property features contain substantial missing values.
- The resulting model applies only to apartments similar to the Munich listings in the dataset.

## Key takeaway

Defining the target, scope, features, and exclusions is part of modelling. Every later metric and conclusion depends on these decisions.

## Related notes

- [Regression Metrics and Baselines](Regression%20Metrics%20and%20Baselines.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)
- [Error Analysis and Feature Effects](Error%20Analysis%20and%20Feature%20Effects.md)