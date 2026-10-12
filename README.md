# Economical Cognitive Impairment Classification Using the AIBL Dataset

An R-based machine-learning research project examining whether a reduced set of cognitive assessments can distinguish healthy controls (HC) from participants with mild cognitive impairment or Alzheimer's disease (Non-HC) in the Australian Imaging Biomarkers and Lifestyle (AIBL) baseline dataset.

> **Research prototype only.** These findings do not establish clinical diagnostic validity and should not be used for patient diagnosis or treatment decisions.

## Highlights

- **628 baseline participants:** 447 HC and 181 Non-HC, as reported in the study.
- **29 candidate predictors** spanning demographic, medical-history, genetic, blood and cognitive variables.
- **Four classifiers:** Random Forest, k-Nearest Neighbours, Gradient Boosting Machine and Naive Bayes.
- **Methods:** missing-data handling, kNN imputation, IQR-based outlier capping, ROSE balancing, PCA, repeated cross-validation and Recursive Feature Elimination (RFE).
- **Reported held-out AUC:** Naive Bayes 98.41%; Gradient Boosting 98.39%; Random Forest 98.37%; kNN 93.80%.
- **Economical configuration:** four cognitive variables (CDR, MMSE, LMIR and LMDR) with a reported AUC of 98.43% in the configuration comparison.

These figures are **results reported by the original analysis**, not independently reproduced for this repository packaging.

## Research question

Can a small, low-cost subset of cognitive measures offer similar discrimination to a model that also includes laboratory and genetic measurements?

## Project files

| Path | Description |
| --- | --- |
| `analysis/aibl_analysis.Rmd` | R Markdown source for preprocessing, exploratory analysis, modelling and figures |
| `reports/aibl_analysis.html` | Pre-rendered analysis, viewable without R |
| `reports/research_paper.docx` | Accompanying academic report |
| `dataset/README.md` | Instructions for obtaining and placing authorised AIBL data |

## Running the analysis

1. Install R and RStudio, and obtain the AIBL data through an authorised source.
2. Place the eight required CSV files in a folder named `dataset/` **beside the R Markdown file's working directory**. The simplest option is to open the repository root as an RStudio project and render from that directory.
3. From the repository root, run:

```r
rmarkdown::render("analysis/aibl_analysis.Rmd", output_dir = "reports")
```

The R Markdown script includes a package-installation check. Running it can take time because it trains multiple models with cross-validation. Package versions and platform differences may affect reproduction.

## Methodology overview

1. Join eight AIBL source tables using participant identifiers and retain baseline visits.
2. Prepare HC versus Non-HC labels and clinical predictors.
3. Explore distributions, correlations and principal components.
4. Train and compare four classification algorithms using repeated cross-validation and a held-out test partition.
5. Use Random Forest-based RFE and evaluate economical feature subsets.

## Important limitations and reproducibility notes

- **Potential preprocessing leakage:** the original notebook imputes and processes some features before splitting into training and test sets. This can inflate estimated held-out performance. A future revision should fit all learned preprocessing on training data only and apply it to the held-out set; feature selection and balancing should likewise remain inside the appropriate training/CV folds.
- **Model comparison is not clinical validation:** the study uses one baseline cohort. Independent external-cohort validation, confidence intervals and calibration assessment are required before clinical conclusions.
- **RFE interpretation:** the plotted peak in RFE accuracy occurs at a larger feature subset than four; the four-feature configuration is presented as an *economical near-peak alternative*, not necessarily the maximum-scoring RFE subset.
- **Cost claims:** fewer laboratory/genetic measurements suggest lower test burden, but a formal cost-effectiveness analysis was not performed.
- **Data access:** raw participant-level AIBL CSV files are intentionally excluded from this public repository. Their use and redistribution remain subject to the original provider's terms.

## Author

Emmanuel Bajimi — MSc Data Science, Ulster University.
