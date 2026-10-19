# Comparative and Explainable Machine Learning for Coronary Artery Disease Classification

**Paper:** *Comparative and Explainable Machine Learning for Coronary Artery Disease Classification*  
**Authors:** Athanasios Kanavos, Theofanis Aravanis, Konstantinos Ioannou  
**Code copyright:** © 2026 Athanasios Kanavos (MIT License)

This repository contains the Python analysis workflow accompanying the manuscript. The workflow implements eight classification approaches for the UCI processed Cleveland Heart Disease dataset, including preprocessing, held-out evaluation, stratified cross-validation, kNN hyperparameter selection, statistical comparisons, repeated neural-network runs, sensitivity analyses, and model interpretation.

**This is a code-focused repository.** Numerical result tables and manuscript PDFs are not distributed here. Executing the script creates fresh result files in a local output directory. The manuscript contains the reported experimental results; the repository is a research implementation, not a certified reproduction of every historical output. Results, especially those involving stochastic training and resampling, should be evaluated with the documented software environment and code settings.

## Contents

```text
Cleveland-CAD-ML-Benchmark/
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── processed.cleveland.data
└── src/
    └── analysis_pipeline.py
```

## Data

The repository includes `data/processed.cleveland.data`, the 303-row processed Cleveland subset from the UCI Heart Disease dataset. The script excludes six incomplete records and uses 297 observations. CAD is encoded as 0 for original outcome 0 and 1 for outcomes 1–4. The source data carry their own CC BY 4.0 license; see `data/README.md`.

**Original dataset citation:** Janosi, A., Steinbrunn, W., Pfisterer, M., & Detrano, R. (1989). *Heart Disease* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C52P4X

## Models and analyses

- Logistic Regression, Decision Tree, k-Nearest Neighbors, Random Forest, Gradient Boosting, Support Vector Machine, Soft-Voting Ensemble, and PyTorch Multilayer Perceptron.
- Same stratified 80/20 partition (seed 42), plus stratified 10-fold cross-validation (seed 42).
- kNN selection among 1–20 neighbors using inner five-fold stratified validation in each outer training fold.
- Training-fold-only categorical encoding and model-specific scaling; internal MLP validation, early stopping, and seeds 43–62 for additional fixed-partition runs.
- Held-out bootstrap confidence intervals for threshold-dependent metrics, Hanley–McNeil AUC confidence intervals, Friedman testing with paired Wilcoxon and Holm adjustment.
- Sensitivity analysis without `ca`; second analysis excluding `ca`, `thal`, `thalach`, `exang`, `oldpeak`, and `slope`.
- Random-Forest and Gradient-Boosting feature importance and Gradient-Boosting SHAP analysis.

## Installation

The manuscript environment reports Python 3.13.5, scikit-learn 1.8.0, PyTorch 2.10.0, and SHAP 0.50.0. Use a compatible Python interpreter and install packages:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows, activate with `.venv\Scripts\activate`.

## Running the complete workflow

From the repository root:

```bash
python src/analysis_pipeline.py --data data/processed.cleveland.data --out outputs
```

The script writes fresh CSV files and figures to `outputs/`. This includes per-fold predictions metrics, cross-validation summaries, held-out metrics, statistical-test outputs, repeated MLP runs, both sensitivity analyses, and SHAP outputs. These outputs are generated locally; `outputs/` is intentionally excluded from version control.

## Scope

The study is an internal methodological benchmark using a small historical dataset, not a validated screening system or clinical decision-support tool. Attributions explain fitted model behavior, not causal effects or clinical utility.

## License and citation

Original software and documentation: MIT License, copyright © 2026 Athanasios Kanavos. Dataset: UCI Heart Disease, CC BY 4.0, separately attributed. See `LICENSE`, `data/README.md`, and `CITATION.cff`. Please cite the accompanying manuscript when published and the original UCI dataset.
