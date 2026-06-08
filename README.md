# 🩺 X-EndoML: Explainable Endometriosis Risk Prediction

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Dataset](https://img.shields.io/badge/Data-Open%20Source-orange.svg)](https://github.com/Tristan2024/endometriosis-dataset)

**X-EndoML** is a machine learning-driven framework for non-invasive endometriosis risk prediction using self-reported symptoms. It integrates consensus-based feature selection, unsupervised structural validation, and TreeSHAP explainability to provide transparent, biologically grounded risk assessments.

> **Paper:** *X-EndoML: A Machine Learning-Driven Framework for Endometriosis Risk Explainable Prediction*  

---

## 📋 Overview

Endometriosis affects ~10% of reproductive-aged women, yet diagnosis is typically delayed by 7–10 years. X-EndoML addresses this gap through a three-stage pipeline:

```
58 symptoms → Feature Selection → 35 symptoms → Quality Assessment → Explainable RF Classifier
                (Chi², MI, ReliefF)        (UMAP, DBSCAN, Q metric)     (TreeSHAP, DCA)
```

### Key Results
- **F1-Score:** 0.936 on hold-out test set (+3.1% over prior work)
- **AUC-ROC:** 0.976 [95% CI: 0.956–0.991]
- **Feature Reduction:** 39.7% (58 → 35 symptoms)
- **Explainability:** Global and patient-level risk justifications via TreeSHAP

---

## 📁 Repository Structure

| Notebook | Description | Paper Section |
|----------|-------------|---------------|
| `Feature_Selection_Step.ipynb` | Consensus-based filter pipeline (Chi², MI, ReliefF, Phi redundancy removal) | §4.1|
| `UMAP.ipynb` | UMAP visualization with Hamming distance for both feature spaces | §5.1 |
| `DBSCAN.ipynb` | Unsupervised clustering metrics (Silhouette, DB Index, Dunn) and quality metric Q | §5.1 |
| `Baseline Models.ipynb` | Multi-classifier benchmark on the full 58-feature space | §5.2 |
| `Baseline 35.ipynb` | Youden-adjusted Random Forest on the reduced 35-feature space | §5.2 |
| `Hyperparams_RandomForest.ipynb` | Hyperparameter tuning and bootstrap confidence intervals | §5.2 |
| `SHAP_Instances.ipynb` | TreeSHAP global/local explainability and misclassification analysis | §5.3 |
| `Clinical_Validation.ipynb` | Clinical utility through Decision Curve Analysis | §5.4 |
| `Related Work.ipynb` | Comparative evaluation against Goldstein et al. (2023) | §5.4 |
| `dataset.xlsx` | Clinical dataset (886 patients, 58 binary symptoms) | §3 |

---

## 📊 Dataset

The dataset contains self-reported symptoms from **886 patients** collected through specialized online communities ([Goldstein & Cohen, 2023](https://github.com/Tristan2024/endometriosis-dataset)):

- **474** patients with confirmed endometriosis (class 1)
- **412** patients without confirmed diagnosis (class 0)
- **58** binary symptom features across 4 categories: pain, gastrointestinal, menstrual/reproductive, and systemic/psychological

---

## ⚙️ Requirements

```
numpy
pandas
scikit-learn
matplotlib
seaborn
shap
umap-learn
openpyxl
```

---

## 🔬 Framework Pipeline

### 1. Feature Selection (§3.1)
- Permutation test (100 permutations, p < 0.01) for MI significance threshold
- Cross-method consensus: Chi² ∩ MI ∩ ReliefF
- Phi-based redundancy removal (φ ≥ 0.70)
- **Result:** 58 → 35 features

### 2. Unsupervised Quality Assessment (§4.1)
- UMAP projection (`n_neighbors=15`, `min_dist=0.1`, `metric='hamming'`, `random_state=42`)
- DBSCAN clustering with data-driven ε (median 5-NN Hamming distance)
- Custom quality metric Q based on 1-NN class boundary analysis

### 3. Supervised Evaluation (§4.2)
- Youden-optimized Random Forest (threshold = 0.538)
- Bootstrap resampling (n=1000) for confidence intervals
- Decision Curve Analysis for clinical utility validation

### 4. Explainability (§4.3)
- TreeSHAP for global feature importance and local patient-level explanations
- Biological interpretation validated against clinical literature

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
