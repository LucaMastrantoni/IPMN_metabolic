# IPMN_metabolic

Code and source data for **Integrative multi-omics and spatial profiling identify circulating metabolic signatures associated with malignant progression in IPMN**

This repository reproduces the validation-cohort analyses of circulating **GLUL, ALDH2, ECHS1 and SCP2**.

## Analysis design

| Step | Cohort | What is done |
|---|---|---|
| Marker selection | Discovery | Done upstream of this repository. |
| Cut-off derivation | Discovery (n = 56) | One cut-off per protein and contrast, maximising Youden's index. |
| Performance | **Validation** (n = 67) | AUROC (95% CI), sensitivity, specificity, PPV, NPV, accuracy and balanced accuracy at the locked discovery cut-offs. |
| CA19-9 | Validation, complete cases (n = 60 / 50) | CA19-9 is available only as a binary clinical call, so it is compared with GLUL as binary calls. |

No performance estimate is derived from the discovery cohort, because the four proteins were selected on that cohort.

**Contrasts**

- **Primary:** HG-IPMN + PDAC vs LG-IPMN (`malignant_PDAC`)
- **Secondary (exploratory):** HG-IPMN vs LG-IPMN, PDAC excluded (`malignant_IPMN`).

## Repository structure

```
IPMN_metabolic/
├── analysis.Rmd        # full analysis
├── analysis.html       # knitted notebook
├── Data/
│   └── Cohorts.xlsx    # source data, sheets "Discovery" and "Validation"
├── output/             # all figures and tables produced by analysis.Rmd
├── session_info.txt    # R and package versions used for the deposited outputs
├── LICENSE
└── README.md
```

## Data dictionary — `Data/Cohorts.xlsx`

One row per patient; sheets `Discovery` (n = 56) and `Validation` (n = 67).
Cohorts are non-overlapping.

| Column | Description |
|---|---|
| `Pt` | Anonymized patient code |
| `Type` | `Low-Grade IPMN`, `High-Grade IPMN` or `PDAC` |
| `Ca19` | CA19-9 clinical call at the routine threshold: `pos`, `neg`, or `miss` (not recorded; treated as missing) |
| `GLUL`, `ALDH2`, `ECHS1`, `SCP2` | Plasma protein levels |

| Cohort | LG-IPMN | HG-IPMN | PDAC | CA19-9 recorded |
|---|---|---|---|---|
| Discovery | 24 | 14 | 18 | 48 |
| Validation | 46 | 9 | 12 | 60 |

## Reproducing the analysis

Requires R (≥ 4.4) with:

```r
install.packages(c("readxl", "dplyr", "ggplot2", "patchwork", "pROC", "cutpointr", "cvms", "writexl", "scales", "rmarkdown"))
```

Then, from the repository root:

```r
rmarkdown::render("analysis.Rmd")
```

All bootstrap procedures use `set.seed(42)`; outputs are deterministic for a
given package version. The deposited outputs were generated with the versions
listed in `session_info.txt`.
