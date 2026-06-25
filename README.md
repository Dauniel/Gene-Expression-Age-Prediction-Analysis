# C&S BIO 100 — Final Project

Predicting chronological **age from human gene-expression data** (GTEx), and asking
how well that signal generalizes across tissues.

**Author:** Daniel Chang
**Course:** C&S BIO 100 (Computational & Systems Biology), UCLA

## Overview

Using bulk RNA-seq TPM data from the [GTEx](https://gtexportal.org) project, this
project builds a regression model that predicts a sample's age from its
gene-expression profile. The pipeline:

1. **Load & clean** the filtered TPM matrix joined with sample/subject phenotypes.
2. **Standardize** the gene matrix and reduce it with **PCA** (first 10 PCs).
3. **Visualize** PC1 vs PC2, colored by age and marked by tissue.
4. **Model** age with **linear regression** (and ElasticNet) on the PCs + sex covariate.
5. **Evaluate** with 5-fold cross-validation (R² and RMSE).
6. **Quantify uncertainty** with a 10,000-iteration **bootstrap** of the CV R²,
   reporting 95% confidence intervals — both overall and **per tissue**.

## Files

| File | Description |
|------|-------------|
| `final_project.ipynb` | Main analysis notebook (run this) |
| `final_project.html` / `final_project.pdf` | Rendered exports of the notebook |
| `C&S BIO 100 Final Project - Google Slides.pdf` | Presentation slides |
| `C&S BIO 100 Peer Review.pdf` | Peer review feedback |
| `subject_phenotypes.txt` | GTEx subject phenotypes (age group, sex) |

## Data (not included in repo)

Two files are too large for GitHub and are git-ignored. Place them in this folder
before running the notebook:

| File | Size | Source |
|------|------|--------|
| `GTEx_Analysis_Filtered_TPM_with_pheno.csv` | ~866 MB | Derived from GTEx TPM matrix + phenotypes |
| `sample_atttributes.txt` | ~37 MB | GTEx sample attributes |

Download the raw GTEx tables from the
[GTEx Portal datasets page](https://gtexportal.org/home/datasets).

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas numpy scikit-learn matplotlib jupyter
jupyter notebook final_project.ipynb
```

## Methods at a glance

- **Dimensionality reduction:** `StandardScaler` → `PCA(n_components=10)`
- **Models:** `LinearRegression`, `ElasticNet`
- **Validation:** `KFold(n_splits=5, shuffle=True)`, scoring R² and RMSE
- **Uncertainty:** 10,000-sample bootstrap of the cross-validated R², 95% CI,
  computed overall and per tissue (tissues with < 40 samples skipped)
