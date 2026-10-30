# Integrative multi-omics and spatial profiling identify circulating metabolic signatures associated with malignant progression in IPMN

Code and source data for "**Integrative multi-omics and spatial profiling identify circulating metabolic signatures associated with malignant progression in IPMN**". (Agostini et al. - Under Review)

This repository reproduces the validation-cohort analyses of circulating **GLUL, ALDH2, ECHS1 and SCP2**.

## Analysis design

| Step | Cohort | What is done |
|---|---|---|
| Marker selection | Discovery | Done upstream. |
| Cut-off derivation | Discovery (n = 56) | One cut-off per protein and contrast, maximising Youden's index. |
| Performance | **Validation** (n = 67) | AUROC (95% CI), sensitivity, specificity, PPV, NPV, accuracy and balanced accuracy at the locked discovery cut-offs. |
| CA19-9 | Validation, complete cases (n = 60 / 50) | CA19-9 as a binary call, compared with GLUL as binary calls. |

No performance estimate is derived from the discovery cohort.

**Contrasts**

- **Primary:** HG-IPMN + PDAC vs LG-IPMN (`malignant_PDAC`)
- **Secondary (exploratory):** HG-IPMN vs LG-IPMN, PDAC excluded (`malignant_IPMN`).

## Repository structure

```
IPMN_metabolic/
├── analysis.Rmd        # full analysis
├── Data/
│   └── Cohorts.xlsx    # source data, sheets "Discovery" and "Validation"
├── output/             # all figures and tables produced by analysis.Rmd
├── session_info.txt    # R and package versions
├── LICENSE
└── README.md
```

## Data dictionary — `Data/Cohorts.xlsx`

One row per patient; sheets `Discovery` (n = 56) and `Validation` (n = 67).
Cohorts are non-overlapping.

| Column | Description |
|---|---|
| `Pt` | Patient code |
| `Type` | `Low-Grade IPMN`, `High-Grade IPMN` or `PDAC` |
| `Ca19` | CA19-9 clinical call at the routine threshold: `pos`, `neg`, or `miss` (missing) |
| `GLUL`, `ALDH2`, `ECHS1`, `SCP2` | Plasma protein levels (SomaScan RFU) |

## Reproducing the analysis

Requires R (≥ 4.4) with:

```r
install.packages(c("readxl", "dplyr", "ggplot2", "patchwork", "pROC", "cutpointr", "cvms", "writexl", "scales", "rmarkdown"))
```
