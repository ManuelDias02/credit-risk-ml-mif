# Credit Risk Modeling with Machine Learning

## Project Overview

This project develops a supervised machine learning pipeline to estimate the probability that a borrower will default on a loan using historical Lending Club data. Predicted default probabilities are translated into concrete lending decisions (approve or reject) using business-oriented threshold optimization.

The project follows best practices taught in the Machine Learning in Finance course, including strict train/validation/test separation, model tuning on validation data only, and a single-use final holdout test evaluation.

## Dataset

- Source: Lending Club loan data
- Training file: `data/lending_club_train.csv`
- Test file: `data/lending_club_test.csv`
- Target variable: `default` (binary)

The test set contains labels but is treated as a final holdout and used exactly once at the end of the project for an unbiased audit. It is not used for model selection, hyperparameter tuning, feature selection, or threshold optimization.

## Repository Structure

```
credit-risk-ml-mif/
├── data/
│   ├── lending_club_train.csv
│   ├── lending_club_test.csv
│   └── Lending_Club_Data_Dictionary.pdf
├── notebooks/
│   └── credit_risk_project.ipynb
├── outputs/
│   ├── model_comparison_validation.csv
│   ├── tuning_random_forest.csv
│   ├── tuning_gradient_boosting.csv
│   ├── confusion_matrix_*.csv
│   ├── approval_rate_curve_*.png
│   ├── ev_curve_*.png
│   ├── final_report.md
│   ├── model_card.md
│   └── outputs_manifest.txt
├── requirements.txt
└── README.md
```

## Methodology

### Exploratory Data Analysis (EDA)

EDA is performed to understand the distribution of borrower and loan characteristics and their relationship to default risk. Both numerical and categorical variables are analyzed using business intuition.

### Feature Engineering

Several economically motivated features are engineered, including affordability ratios, leverage proxies, and nonlinear transformations. More than four engineered features are created and justified.

### Train / Validation / Test Strategy

Training data is split into train and validation sets using stratification.

The validation set is used for:
- model comparison,
- hyperparameter tuning,
- feature ablation decisions,
- threshold optimization.

The test set is used once, at the end, for final evaluation only.

### Models

The following models are implemented:
- Logistic Regression (baseline, not graded)
- Decision Tree (optional extension)
- Random Forest (final selected model)
- Gradient Boosting (rubric model, not selected)

Model comparison is based on validation ROC-AUC and RMSE, with explicit monitoring of overfitting gaps.

## Final Model

- Model: Random Forest (scikit-learn)
- Feature set: FeatureSet_B (grade and sub_grade removed)
- Class imbalance handling: class_weight = balanced

### Tuned Hyperparameters

- n_estimators = 600
- max_depth = 10
- min_samples_split = 10
- min_samples_leaf = 50
- max_features = sqrt
- class_weight = balanced
- bootstrap = True

## Threshold Optimization

Predicted probabilities are converted into lending decisions using threshold optimization under conservative cost-benefit assumptions, where defaults are more costly than missed approvals.

Chosen threshold: 0.58 (selected on validation data)

The threshold is locked and not re-optimized on the test set.

A Policy Metrics Summary table compares validation and test outcomes at thresholds 0.50 and 0.58.

## Final Holdout Test Results

- Test AUC: 0.707
- Test RMSE: 0.450

Performance on the test set is consistent with and slightly stronger than validation performance, indicating good generalization.

## Reproducibility

The entire workflow is contained in a single notebook.

All derived artifacts are exported to the `outputs/` directory.

An outputs manifest is provided to ensure deterministic reproducibility.

## How to Run

- Install dependencies using `requirements.txt`.
- Open and run `notebooks/credit_risk_project.ipynb`.

## Notes

This project is an academic exercise and does not constitute financial or investment advice.
