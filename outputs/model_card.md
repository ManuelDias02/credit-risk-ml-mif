# Model Card

## Executive Summary
- Predicts probability of default to support lending decisions.
- Final model selected by highest validation AUC for generalization.
- Threshold policy balances approval volume and default risk.
- Decisioning is grounded in transparent payoff assumptions.

## Data & Split
- Train rows: 7596
- Validation rows: 2533
- Test rows: 2533
- Target prevalence (train): 0.1929
- Target prevalence (validation): 0.1927
- Random state: 42

## Feature Engineering
- Affordability: installment_to_income, loan_to_income
- Leverage: revol_balance_util, revol_balance_to_income
- Stability/experience: open_to_total_acc, recent_inquiry_flag
- Nonlinear: log_annual_inc, sqrt_dti

## Models Compared
Model performance is summarized in `model_comparison_validation.csv`. The final model is selected based on validation AUC, with AUC gap used to monitor overfitting risk.

## Final Model

- **Model**: Random Forest (sklearn)
- **Feature set**: FeatureSet_B (grade / sub_grade removed)
- **Preprocessing**: Same pipeline as the notebook (OHE for categoricals, imputation for missing values)
- **Hyperparameters**:
  - n_estimators = 600
  - max_features = 'sqrt'
  - max_depth = 10
  - min_samples_split = 10
  - min_samples_leaf = 50
  - class_weight = 'balanced'
  - bootstrap = True

## Final Metrics

- **Validation AUC**: 0.695576 (FeatureSet_B)
- **Test AUC**: 0.707122
- **Test RMSE**: 0.450406
- **Threshold used on test**: 0.58 (validation-chosen; not re-optimized)

## Decision Policy

- **Thresholds reported**: 0.50 and 0.58
- **Approval rate @ 0.50**: 0.652981
- **Default rate among approved @ 0.50**: 0.125756
- **Approval rate @ 0.58**: 0.833004
- **Default rate among approved @ 0.58**: 0.153081

## Final Policy (Validation)

- **Final model**: Random Forest
- **Validation AUC**: 0.695576
- **Validation RMSE**: 0.4625
- **Optimal threshold**: 0.58
- **Approval rate (validation)**: 0.786
- Expected value: -21

## Confusion Matrices (Validation)
- Threshold 0.50: approvals=1528, defaults among approved=182, approval_rate=0.603
- Optimal threshold: approvals=1992, defaults among approved=280, approval_rate=0.786
- Full tables are saved in `confusion_matrix_validation_050.csv` and `confusion_matrix_validation_optimal.csv`.

## Assumptions & Interpretation
The decision policy is based on stylized payoff assumptions (e.g., -5 for a default and +1 for a successful repayment). Under these assumptions, the optimal threshold is chosen to minimize expected loss (or maximize expected value). It is important to note that the resulting Expected Value (EV) may be negative if the baseline default rate is high or if the model’s discriminative power is limited. The sensitivity analysis (`threshold_sensitivity.csv`) further explores how the optimal threshold and corresponding EV shift under different cost-benefit calibrations, highlighting the dependence of the policy on these business assumptions.

## Limitations & Next Steps
- Payoff assumptions are stylized; calibrate to bank economics.
- Threshold chosen on validation; test remains untouched for final audit.
- Monitor drift and recalibrate thresholds as conditions change.
- Consider calibration and fairness analyses as follow-on work.

## Reproducibility
- Run `notebooks/credit_risk_project.ipynb` → Restart Kernel & Run All.
