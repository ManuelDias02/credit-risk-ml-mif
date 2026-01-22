## Credit Risk ML Project

This repository contains a single, end-to-end notebook for a supervised credit risk (PD) modeling workflow. The notebook is fully reproducible and produces a deterministic set of outputs for grading and review.

### How to Run
- Open `notebooks/credit_risk_project.ipynb`.
- Restart Kernel & Run All.
- The notebook reads only:
  - `data/lending_club_train.csv`
  - `data/lending_club_test.csv`

### Outputs Policy
Outputs are regenerated on Run All and overwrite existing artifacts; commit outputs only for the final snapshot; manifest is deterministic.

All outputs are generated deterministically by the notebook and written to `outputs/`.
The export step produces a manifest (`outputs/outputs_manifest.txt`) that records:
- git commit hash
- file list, sizes, SHA256 hashes, and brief purpose

Expected artifacts include model comparison tables, tuning tables, threshold sweep outputs, decisioning curves, and a model card. The outputs are derived artifacts only (no raw data), and the manifest is the single source of truth for what was exported in a given run.

## Running and Inspecting the Project in Google Colab (Code + Outputs)

While the `outputs/` folder contains the final generated results, the full logic, detailed explanations, and Python code live in `notebooks/credit_risk_project.ipynb`. To see and interact with the code, you should open the notebook in Google Colab rather than just browsing it on GitHub.

### Option 1 — One-click Colab links (recommended)
You can open the project in Colab using these links:
- **[Main Branch](https://colab.research.google.com/github/Jannikwendt/credit-risk-ml-mif/blob/main/notebooks/credit_risk_project.ipynb)**: The most recent version of the project.
- **[Submission Snapshot (submission-v1)](https://colab.research.google.com/github/Jannikwendt/credit-risk-ml-mif/blob/submission-v1/notebooks/credit_risk_project.ipynb)**: An immutable, graded version of the submission. Guarantees a frozen snapshot.

### Option 2 — Manual Colab UI steps
1. Go to [https://colab.research.google.com](https://colab.research.google.com)
2. Click **File** → **Open notebook**
3. Select the **GitHub** tab
4. Paste this repository URL: `https://github.com/Jannikwendt/credit-risk-ml-mif`
5. Select `notebooks/credit_risk_project.ipynb` from the list
6. Click **Open Notebook**

### How to Make Sure Code Cells Are Visible
If the notebook opens and you only see the text and charts (outputs):
- Click **View** → **Expand sections** in the top menu.
- This ensures all Python code cells are visible and not collapsed.
- The notebook contains extensive markdown explanations above each code section to guide you through the modeling logic.

### Final Execution Instructions
To regenerate all results and charts from scratch:
- Click **Runtime** → **Restart runtime**
- Click **Runtime** → **Run all**
- **Note**: All outputs will be regenerated and will overwrite any existing artifacts in the `outputs/` folder within your Colab environment.

## Run on Google Colab (Reproducibility Test)

This section explains exactly how to reproduce the project outputs from scratch using Google Colab. Colab is a free cloud environment that requires no installation on your computer.

### 1. Open Google Colab
Go to: [https://colab.research.google.com](https://colab.research.google.com)

### 2. Clone the repository in Colab
In a new Colab cell, copy and paste the following commands to download the project code:

```bash
!git clone https://github.com/Jannikwendt/credit-risk-ml-mif.git
%cd credit-risk-ml-mif
!git fetch --all --tags
!git checkout submission-v1
```

**Why `submission-v1`?**
We use the `submission-v1` tag because it is a frozen, graded snapshot of the project. This guarantees you are using the exact same notebook and logic as the submitted version.

### 3. Upload the required data files
The notebook only requires two data files:
- `lending_club_train.csv`
- `lending_club_test.csv`

In a new cell, run this to upload the files from your computer:

```python
from google.colab import files
files.upload()
```

Then, move the uploaded files into the correct folder:

```bash
!mkdir -p data
!mv lending_club_train.csv data/
!mv lending_club_test.csv data/
```

*Note: No other data files are needed. The PDF data dictionary is optional and not used by the code.*

### 4. Install dependencies
Run this command to install the exact versions of the libraries used in this project:

```bash
!pip install -r requirements.txt
```

### 5. Run the notebook
You have two options to run the analysis:

**Option A (Recommended – Fully Automated):**
This simulates a clean "Restart Kernel & Run All" and ensures full reproducibility.

```bash
!jupyter nbconvert --to notebook --execute notebooks/credit_risk_project.ipynb \
  --output executed_credit_risk_project.ipynb \
  --ExecutePreprocessor.timeout=1200
```

**Option B (Manual):**
1. Navigate to the file browser on the left in Colab.
2. Open `notebooks/credit_risk_project.ipynb`.
3. Click **Runtime** → **Restart and run all**.

### 6. Verify outputs
All results are written to the `outputs/` folder. You can list the files and view the manifest:

```bash
!ls outputs
!cat outputs/outputs_manifest.txt
```

**Note:** `outputs_manifest.txt` is the single source of truth. It lists all generated artifacts, their sizes, and their unique SHA256 "digital fingerprints" (hashes).

### 7. What "success" looks like
- The notebook runs to completion without errors.
- The `outputs/` folder contains all expected CSV, PNG, and Markdown files.
- `final_report.md` exists and summarizes the results in human-readable form.
- `outputs_manifest.txt` confirms that all files were generated deterministically.

### 8. Important notes
- **Fresh Generation**: Outputs are regenerated and overwritten every time you "Run All".
- **Input Isolation**: Only the two provided CSV files are used as inputs.
- **No Data Leakage**: The `lending_club_test.csv` file is an unlabeled hold-out set and is never used for model training or tuning.
