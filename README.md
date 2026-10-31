# Design of Alloys for High Strength-to-Weight Ratio

Machine learning models that predict the **maximum stress** and **minimum density** of aluminium (Al) alloys from their composition, and rank new candidate alloys using uncertainty-aware **Expected Improvement (EI)**.

> Course project for **Material Informatics** (Semester 2) — Team B7.
> Team members: Jeevakamal K R , Shatha Varsha Sree T , Guhan K B , Gowtham S D

## Overview

Lightweight, strong alloys need a high strength-to-weight ratio. Experiments are slow and expensive, so this project trains surrogate models on existing Al alloy data and uses them to screen a large pool of virtual compositions. Two separate models are built:

| Target | Notebook |
|---|---|
| Maximum stress (MPa) | `maxStress_prediction_model.ipynb` |
| Minimum density (g/cm³) | `Density_prediction_model.ipynb` |

[![Open Density notebook in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/kamaladhi/High-Strength-Lightweight-Alloys/blob/main/Density_prediction_model.ipynb)
[![Open MaxStress notebook in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/kamaladhi/High-Strength-Lightweight-Alloys/blob/main/maxStress_prediction_model.ipynb)

## Features

- **Property prediction:** Support Vector Regression (SVR) for maximum stress and minimum density.
- **Composition-based featurization:** features generated from alloy formulas with the [CBFV](https://github.com/kaaiian/CBFV) library, using a custom element-property table (`f3_revised.csv`).
- **Preprocessing:** 90% train / 10% test split (`random_state=42`) and feature scaling with `StandardScaler`.
- **Hyperparameter tuning:** `GridSearchCV` with `GroupKFold` (5 splits). Rows with the same alloy composition stay in the same fold during cross-validation, which prevents leakage between folds.
- **Uncertainty quantification:** bootstrapping (1000 resamples) gives a mean and standard deviation for every prediction.
- **Expected Improvement:** an acquisition function from Bayesian optimization, used to rank promising candidate compositions.

## Repository Structure

```
.
├── Density_prediction_model.ipynb      # minimum density model
├── maxStress_prediction_model.ipynb    # maximum stress model
└── README.md
```

### Input files required by the notebooks

The notebooks expect these files, which are **not** all stored in this repository. Add them to the repo or upload them to Colab when prompted.

| File | Used for |
|---|---|
| `maxStress_target.xlsx` | Training/test data for maximum stress (`formula`, `target` columns) |
| `Density_target.xlsx` | Training/test data for density (`formula`, `target` columns) |
| `Al-Vitual-Samples.xlsx` | Virtual candidate compositions to screen (`formula`, `target` columns). Spelling matches the filename the notebooks read. |
| `f3_revised.csv` | Custom element-property table used by CBFV for featurization |

## Getting Started

The notebooks were written for **Google Colab** (they use `/content/...` paths and `google.colab.files.upload()`), so Colab is the easiest way to run them.

### Option A: Google Colab (recommended)

1. Click one of the **Open in Colab** badges above.
2. Run the install cell (`!pip install CBFV`).
3. Upload the input files listed above when prompted, or place them in `/content/`.
4. Run all cells in order.

If the cell that moves `f3_revised.csv` fails, because the hardcoded `python3.10` path doesn't exist on your runtime, replace it with:

```python
import os, shutil, CBFV
shutil.move('f3_revised.csv', os.path.join(os.path.dirname(CBFV.__file__), 'element_properties'))
```

### Option B: Run locally

Prerequisites: Python 3.10+ and Jupyter.

```bash
git clone https://github.com/kamaladhi/High-Strength-Lightweight-Alloys.git
cd High-Strength-Lightweight-Alloys
pip install pandas numpy "scikit-learn<1.6" matplotlib seaborn scipy openpyxl CBFV
jupyter notebook
```

Then, before running the notebooks:

- Put the four input files in the project folder.
- Change the `/content/...` paths in the notebooks to local paths.
- Remove the `google.colab` upload cell and copy `f3_revised.csv` into CBFV's `element_properties` folder using the snippet above.

> `scikit-learn<1.6` is needed because the notebooks call `mean_squared_error(..., squared=False)`, which was removed in newer versions.

## What the notebooks do

1. Load the target `.xlsx` file and split it 90/10 into train and test sets.
2. Generate CBFV features from the alloy formulas and scale them.
3. Tune and train an SVR with `GridSearchCV` + `GroupKFold`.
4. Evaluate with MAE, RMSE and R², and plot predicted vs. actual values.
5. Estimate prediction uncertainty by bootstrapping.
6. Retrain on all available data with the same hyperparameters, then score the virtual candidates from `Al-Vitual-Samples.xlsx` (the first 1000 rows).
7. Compute Expected Improvement and save results to CSV (`max_stress_pred.csv` or `Denisty_prediction.csv`).

## Models and Metrics

Both models use SVR with a linear kernel (the best configuration found by grid search).

### Maximum Stress

| | Value |
|---|---|
| Best hyperparameters | C = 10, epsilon = 0.5, kernel = linear |
| Train R² | ~0.729 |
| Test R² | ~0.668 |
| Test MAE | ~488.77 MPa |
| Test RMSE | ~597.99 MPa |

### Minimum Density

| | Value |
|---|---|
| Best hyperparameters | C = 0.1, epsilon = 0.01, kernel = linear |
| Train R² | ~0.947 |
| Test R² | ~0.910 |
| Test MAE | ~0.323 g/cm³ |
| Test RMSE | ~0.519 g/cm³ |

## Known Limitations

- **Density EI direction:** the EI calculation measures improvement over the current *maximum* observed density. For true density minimization it should be adapted to look for compositions that *reduce* density, or replaced with a Lower Confidence Bound (LCB) acquisition function.
- **Train/test split:** the 90/10 split is random, not grouped by composition, so duplicate formulas can appear on both sides. Only the cross-validation is grouped.
- **Candidate screening:** only the first 1000 virtual candidates are scored.
- **Dataset size:** the max stress model has a noticeably lower test R² than the density model, so its predictions should be treated as rough guidance.

## Acknowledgements

- [CBFV](https://github.com/kaaiian/CBFV) for composition-based feature vectors
- Course: Material Informatics, Semester 2


