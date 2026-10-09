# Product Strength Prediction

[View the modelling notebook](notebooks/modelling.ipynb)

Machine-learning analysis of manufacturing, quality-control, and maintenance data to predict cured product strength and identify factors associated with strength variation, supporting both earlier estimation and future process improvement.

This project uses anonymised R&D and manufacturing data from Concrete Canvas Ltd. Data, feature names, and commercially sensitive process details have been removed or anonymised.

This is a personal portfolio project and is not an official publication of Concrete Canvas Ltd.

## Project Overview

Direct measurement of cured product flexural strength is delayed by curing, sample preparation, and technician availability. Strength also arises from a complex production process, making the factors associated with variation difficult to isolate. This project therefore addresses two objectives: providing an earlier indication of product strength and improving understanding of the factors associated with strength variation.

The analysis combines product, manufacturing, and maintenance information and compares linear and non-linear regression approaches using cross-validation. Model explainability techniques are used to identify influential features and relationships that can guide targeted production trials and process-improvement hypotheses.

The target is expressed as a relative **strength index**, where **100 represents the mean observed flexural strength** in the modelling dataset. This preserves the relative relationships between target values without exposing absolute strength measurements.

## Modelling Approach

The notebook compares 10 model configurations, including:

- Linear, Lasso, and Ridge regression
- Standardised and unstandardised linear models
- Decision tree
- Random Forest
- Stochastic Gradient Boosting
- Multilayer Perceptron neural network

Median imputation is performed within each modelling pipeline to avoid preprocessing leakage. Standardisation is applied where appropriate.

Hyperparameters are assessed using a shared shuffled **5-fold cross-validation** partition, followed by comparison of the selected models using a shared shuffled **10-fold cross-validation** partition.

Performance is evaluated using **R²** and **RMSE**.

## Key Results

**Random Forest produced the strongest cross-validated performance:**

- Mean CV R²: **0.527 ± 0.117**
- Mean fold RMSE: **15.133 strength-index points**

The strongest linear comparison, standardised Lasso, achieved:

- Mean CV R²: **0.423 ± 0.144**
- Mean fold RMSE: **16.657 strength-index points**

The Random Forest fitted the training data substantially more closely than it generalised under cross-validation (training R² **0.938** versus mean CV R² **0.527**), indicating overfitting and motivating more rigorous future validation.

## Model Interpretation

The selected Random Forest is interpreted using **SHAP**, while standardised Lasso coefficients provide a complementary linear perspective.

The leading Random Forest features by mean absolute SHAP contribution were:

1. `stitching_measurement`
2. `sample_mass`
3. `setting_rate`

SHAP analysis shows non-linear relationships and variation in feature contributions across observations. Lasso provides a simpler comparison of the direction and relative magnitude of fitted linear associations.

These explanations identify relationships and potential areas for further investigation, but should not be interpreted as causal effects.

## Limitations and Next Steps

The main limitations are:

- **Overfitting:** Random Forest training performance is substantially stronger than cross-validation performance.
- **Selection bias:** Hyperparameter tuning and model comparison use the same dataset.
- **Temporal generalisation:** Shuffled cross-validation does not directly test performance on later production data or under changing process conditions.

The most important next steps are validation on future production data and targeted production trials to test whether model-identified relationships can support improvements in product strength.

## Repository Structure

```text
product-strength-ml/
├── notebooks/
│   └── modelling.ipynb
├── LICENSE
├── requirements.txt
└── README.md
```

The source data are intentionally excluded from the public repository. Proprietary data-integration and feature-engineering procedures are also omitted.

As a result, the notebook is provided primarily as a record of the modelling workflow, results, and interpretation rather than as a fully reproducible analysis from public data.

## Tools

- Python 3.12
- pandas
- NumPy
- scikit-learn
- Matplotlib
- SHAP
- Jupyter

Exact package versions are listed in [`requirements.txt`](requirements.txt).

## License

This project is released under the [MIT License](LICENSE).