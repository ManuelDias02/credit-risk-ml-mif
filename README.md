## Credit Risk ML Project

This repository contains a single, end-to-end notebook for a supervised credit risk (PD) modeling workflow. The notebook is fully reproducible and produces a deterministic set of outputs for grading and review.

### Project Objective
Estimate Probability of Default (PD) using supervised ML techniques from class and translate those probabilities into lending decisions using a business-oriented threshold.

### Methodology Overview
- Exploratory Data Analysis with business interpretation
- Economically motivated feature engineering
- Stratified train/validation split with test holdout
- Models: Logistic Regression, Decision Tree, Random Forest, Gradient Boosting
- Model selection via validation AUC and overfitting checks
- Business-driven threshold optimization using expected value

### How to Run
- Open `notebooks/credit_risk_project.ipynb`.
- Restart Kernel & Run All.
- The notebook reads only:
  - `data/lending_club_train.csv`
  - `data/lending_club_test.csv`

### Outputs Policy
All outputs are generated deterministically by the notebook and written to `outputs/`.
The export step produces a manifest (`outputs/outputs_manifest.txt`) that records:
- timestamp
- git commit hash
- file list, sizes, and brief purpose

Expected artifacts include model comparison tables, tuning tables, threshold sweep outputs, decisioning curves, and a model card. The outputs are derived artifacts only (no raw data), and the manifest is the single source of truth for what was exported in a given run.

### Grading & Reproducibility Notes
- The notebook is the only required deliverable.
- No test data is used for fitting or tuning.
- Results are reproducible via “Restart Kernel & Run All”.
- `outputs/` contains derived artifacts only.
