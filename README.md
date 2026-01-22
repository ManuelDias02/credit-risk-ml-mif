## Credit Risk ML Project

This repository contains a single, end-to-end notebook for a supervised credit risk (PD) modeling workflow. The notebook is fully reproducible and produces a deterministic set of outputs for grading and review.

# Reviewer Manual: Running and Inspecting the Project

This project is designed to be fully transparent and reproducible. This manual explains how to view the underlying Python code and regenerate all results using Google Colab.

## 1. View and Inspect the Code

Reviewers can view the project in three different ways. Please note that **GitHub previews may collapse or hide code cells** to focus on results. To see the full implementation, comments, and explanations, we recommend opening the notebook in Google Colab.

### Option A: One-click Colab links (Recommended)
These links open the notebook directly in the Google Colab environment:
- **[Submission Snapshot (submission-v1)](https://colab.research.google.com/github/Jannikwendt/credit-risk-ml-mif/blob/submission-v1/notebooks/credit_risk_project.ipynb)**: Use this for grading. It is a frozen, immutable snapshot of the project.
- **[Main Branch](https://colab.research.google.com/github/Jannikwendt/credit-risk-ml-mif/blob/main/notebooks/credit_risk_project.ipynb)**: The most recent "live" version of the code.

**⚠️ Important: Opening the notebook does not include the data files. You must upload the CSVs before running (see the Mandatory step below).**

### Required Step: Upload Data Files (Mandatory)
Google Colab provides the code environment, but it does not automatically include the dataset files when you open a single notebook via a link. The notebook is designed to check for these files immediately to ensure valid results.

**Why this error appears (AssertionError / FileNotFoundError):**
If you try to run the first few cells before uploading, you will see an error. This is **expected and correct**. It is a safety feature to prevent the model from running on missing or incorrect data.

To fix this and run the project, run this code in a new cell at the top of the notebook:

```python
from google.colab import files
files.upload()
```
Upload `lending_club_train.csv` and `lending_club_test.csv`. Then run these commands in a cell to move them into the expected folder:

```bash
!mkdir -p data
!mv lending_club_train.csv data/
!mv lending_club_test.csv data/
```
These two CSV files are the **only required inputs** for the project to function.

### How to Ensure Code Cells Are Visible
If the notebook opens and you only see text and charts:
1. In the top menu, click **View** → **Expand sections**.
2. This will ensure all Python code cells, internal comments, and logic blocks are visible.
3. Above every code cell, you will find a "Goal/Inputs/Outputs" markdown block explaining exactly what that step does.

---

## 2. Reproduce the Outputs Exactly (Google Colab)

Follow these steps to regenerate all project artifacts from scratch in the Colab cloud environment.

### Step 1: Prepare the Environment
In a new Colab code cell, copy and run these commands to download the frozen project snapshot:

```bash
!git clone https://github.com/Jannikwendt/credit-risk-ml-mif.git
%cd credit-risk-ml-mif
!git fetch --all --tags
!git checkout submission-v1
```
*Note: The `submission-v1` tag guarantees you are running the exact logic and notebook version that was submitted.*

### Step 2: Upload the Data Files
The project requires two input files. In a new cell, run:

```python
from google.colab import files
files.upload()
```
Upload `lending_club_train.csv` and `lending_club_test.csv` from your computer. Then, move them to the data folder:

```bash
!mkdir -p data
!mv lending_club_train.csv data/
!mv lending_club_test.csv data/
```

### Step 3: Install Dependencies
Run this to install the specific library versions used in the project:

```bash
!pip install -r requirements.txt
```

### Step 4: Execute and Regenerate
Run all cells to regenerate the `outputs/` folder. You can do this manually (**Runtime** → **Run all**) or via the command line to simulate a fresh environment:

```bash
!jupyter nbconvert --to notebook --execute notebooks/credit_risk_project.ipynb \
  --output executed_credit_risk_project.ipynb \
  --ExecutePreprocessor.timeout=1200
```

---

## 3. Verify Success

Once the execution is finished, follow this checklist to verify the results:

- [ ] **No Errors**: The notebook ran to completion without any red error messages.
- [ ] **Outputs Generated**: Running `!ls outputs` shows a full list of CSV, PNG, and Markdown files.
- [ ] **Final Report**: The file `outputs/final_report.md` exists and provides a human-readable executive summary of the results.
- [ ] **Reproducibility Proof**: The file `outputs/outputs_manifest.txt` lists all generated files along with their SHA256 "digital fingerprints."
- [ ] **Deterministic Results**: The performance metrics (AUC, RMSE) match those described in the `final_report.md`.

### Important Notes
- **Input Isolation**: The `lending_club_test.csv` is a hold-out set and is **never** used for training or tuning.
- **Overwriting**: Every "Run All" execution completely regenerates and overwrites the artifacts in the `outputs/` folder.
