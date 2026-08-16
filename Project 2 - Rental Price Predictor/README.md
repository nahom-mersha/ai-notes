# Rental Price Predictor

Notes from Project 2 of my AI engineering roadmap.

This project includes an end-to-end machine-learning workflow that predicts monthly cold rent for apartments in Munich. It combines a learning-focused NumPy implementation of linear regression with professional scikit-learn pipelines, model evaluation, error analysis, SQL examples, a prediction CLI, and a deployed Streamlit application.

- Project repository: https://github.com/nahom-mersha/rental-price-predictor
- Live application: https://munich-rent-predictor.streamlit.app/

## AI assistance

This is an AI-assisted learning project. I used ChatGPT to help generate and explain code, then reviewed the implementation, ran tests, explored the underlying concepts, and documented what I learned.

## What the project covers

- Supervised learning and regression
- Dataset selection, cleaning, and target definition
- Regression metrics and a mean-price baseline
- Linear regression from scratch with NumPy
- Gradient descent, feature scaling, and closed-form solutions
- Leakage-safe preprocessing with scikit-learn pipelines
- Cross-validation, Ridge regularization, and decision-tree comparison
- Residual analysis, outlier investigation, and feature effects
- SQL analysis with SQLite and Pandas
- Model serialization, input validation, CLI prediction, and Streamlit deployment
- Model limitations and responsible use

## Notes

- [Rental Prediction Problem and Data Preparation](Rental%20Prediction%20Problem%20and%20Data%20Preparation.md)
- [Regression Metrics and Baselines](Regression%20Metrics%20and%20Baselines.md)
- [Linear Regression from Scratch with NumPy](Linear%20Regression%20from%20Scratch%20with%20NumPy.md)
- [Gradient Descent, Scaling, and Closed-Form Solutions](Gradient%20Descent%2C%20Scaling%2C%20and%20Closed-Form%20Solutions.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)
- [Cross-Validation, Regularization, and Model Selection](Cross-Validation%2C%20Regularization%2C%20and%20Model%20Selection.md)
- [Error Analysis and Feature Effects](Error%20Analysis%20and%20Feature%20Effects.md)
- [SQL for Rental Data Analysis](SQL%20for%20Rental%20Data%20Analysis.md)
- [Model Serving with CLI and Streamlit](Model%20Serving%20with%20CLI%20and%20Streamlit.md)

## Key takeaway

A useful machine-learning project requires more than training a model. The prediction problem, data, preprocessing, evaluation, error patterns, deployment interface, and limitations all need to be understood together.