# Decoding-Environmental-Status-w-eDNA-and-ML
This repository provides Python code and data to reproduce results and figures from the paper:  Steel Pascual, L.M., Fehling, M., Annandale, D., Ricciardi, F., &amp; Mangale, C. (2026) "Decoding Environmental Status with eDNA and Supervised Machine Learning: Insights into Ecological Condition and Species‑Level Predictors in Solomon Island Rivers".

---

**Author**: Laura Steel Pascual  
**Date**: February 2026  
**Project**: Decoding Environmental Status with eDNA and Supervised Machine Learning: Insights into Ecological Condition and Species‑Level Predictors in Solomon Island Rivers

---

## Overview

This project applies supervised machine learning to environmental DNA (eDNA) metabarcoding data to classify river sites as reference (minimally disturbed) or impacted (anthropogenically disturbed) across eight rivers on Guadalcanal, Solomon Islands. eDNA samples were collected from 47 sites in August 2024 and processed using COI metabarcoding, yielding species-level read counts for 861 taxa.

Site classifications are derived from Land Use Intensification (LUI) scores calculated from Sentinel-2 land-use/land-cover imagery (2017–2023) across four buffer distances (100 m, 300 m, 500 m, 1000 m) and two disturbance thresholds (0.03125, 0.0625), producing eight buffer–threshold configurations for model training and evaluation.

Four classifier types (Random Forest, XGBoost, Decision Tree, Logistic Regression) are compared, with Random Forest selected for hyperparameter tuning and statistical validation (permutation testing). Feature importance is assessed using Gini impurity and SHAP (SHapley Additive exPlanations) values to identify species-level indicator of ecological condition.

---

## Notebooks

PDF versions of all notebooks are available in `pdfs/` if you would prefer to read the analysis without running any code.

The notebooks are designed to be run in the following order:

```
1a_LUI_Thresholds
        ↓
1_Data_Preprocessing
        ↓
2_Model_Training  ───── 2a_CV_Fold_Comparison  (methodological justification, optional)
        ↓           └── 2b_Binary_vs_Counts_Comparison  (methodological justification, optional)
3_Permutation_Tests
        ↓
4_Feature_Importance
```

**Notebooks 4 can also be run independently from Notebook 3**, provided the preprocessed datasets (`results/preprocessed/`) and trained models (`results/models/`) from Notebooks 1 and 2 are already present.

> **Note on runtime:** Notebook 3 (Permutation Tests) runs 1,000 permutations per model (8 models total). Depending on your machine's processing power, this can take anywhere from **1 to 12 hours**. It is recommended to run this notebook when you can leave it unattended.

---

### Notebook descriptions

| Notebook | Description |
|----------|-------------|
| `1a_LUI_Thresholds` | Explores LUI distributions across sites, years, and buffer distances to select the two classification thresholds (0.03125 and 0.0625) used throughout the study |
| `1_Data_Preprocessing` | Loads and prepares eDNA species count data; combines with LUI-derived site classifications to produce eight model-ready datasets (4 buffers × 2 thresholds) |
| `2_Model_Training` | Benchmarks four classifiers using 5-fold stratified cross-validation; tunes Random Forest hyperparameters via GridSearchCV; saves optimised model pipelines |
| `2a_CV_Fold_Comparison` | Compares 10-fold vs 5-fold CV stability to justify the choice of 5-fold CV for the small sample size (n = 47) |
| `2b_Binary_vs_Counts_Comparison` | Compares classification performance using binary presence/absence vs read count features; justifies retention of count data for SHAP interpretability |
| `3_Permutation_Tests` | Validates all eight optimised Random Forest models via 1,000-permutation tests; identifies the seven statistically significant models (p < 0.05) |
| `4_Feature_Importance` | Calculates Gini importance and SHAP values for the seven significant models; identifies 61 key indicator species via cross-model consistency analysis; outputs 61 SHAP dependence plots |

---

## Results Structure

```
results/
├── preprocessed/        # Model-ready datasets (pickle) and species list
├── models/              # Trained Random Forest pipelines (pickle)
├── model_performance/   # CV summary tables and variance analyses (Excel)
├── permutation_tests/   # Permutation test results and null distribution plots
├── feature_importance/  # Gini and SHAP importance tables per model (Excel)
├── shap_analysis/       # SHAP beeswarm plots, comparison charts, dependence plots
└── common_species/      # Cross-model indicator species summary (Excel)
```

---

## Dependencies

```
numpy
pandas
scikit-learn
xgboost
shap
matplotlib
seaborn
openpyxl
```

Install all at once:

```bash
pip install numpy pandas scikit-learn xgboost shap matplotlib seaborn openpyxl
```
