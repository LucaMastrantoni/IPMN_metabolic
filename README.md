# IPMN_metabolic

Code and anonymized source data for the plasma-protein analyses of:

> **Integrative multi-omics and spatial profiling identify circulating metabolic
> signatures associated with malignant progression in IPMN**
> Agostini A, Priori L, Nista EC, … Tortora G, Carbone C.

This repository reproduces the validation-cohort analyses of circulating
**GLUL, ALDH2, ECHS1 and SCP2** (main **Figure 5**, **Supplementary Figure 5**
and **Supplementary Table 6**).

## Analysis design

| Step | Cohort | What is done |
|---|---|---|
| Marker selection | Discovery | Done upstream of this repository (plasma differential abundance → spatial transcriptomics → tissue validation; see manuscript). Not re-run here. |
| Cut-off derivation | Discovery (n = 56) | One cut-off per protein and contrast, maximising Youden's index (rule: marker ≥ cut-off → positive). If a protein separates the classes completely, the midpoint of the separation gap is used. |
| Performance | **Validation only** (n = 67) | AUROC (95% CI, 2,000 stratified bootstrap replicates); sensitivity, specificity, PPV, NPV, accuracy (Clopper–Pearson 95% CI) and balanced accuracy (bootstrap 95% CI) at the **locked** discovery cut-offs. |
| CA19-9 | Validation, complete cases (n = 60 / 50) | CA19-9 is available only as a binary clinical call, so it is compared with GLUL as binary calls (no ROC-vs-ROC comparison), plus an exploratory "either-positive" rule (GLUL or CA19-9 positive). |

No multivariable model is fitted, and no performance estimate is derived from
the discovery cohort, because the four proteins were selected on that cohort.

**Contrasts**

- **Primary:** HG-IPMN + PDAC vs LG-IPMN (`malignant_PDAC`)
- **Secondary (exploratory):** HG-IPMN vs LG-IPMN, PDAC excluded (`malignant_IPMN`). There are 9 HG-IPMN cases in the validation cohort, so these estimates are imprecise.

## Repository structure

```
IPMN_metabolic/
├── analysis.Rmd        # full analysis (knit from the repository root)
├── analysis.html       # knitted notebook (tables and plots, no R needed)
├── Data/
│   └── Cohorts.xlsx    # anonymized source data, sheets "Discovery" and "Validation"
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
| `Ca19` | CA19-9 clinical call at the routine threshold of each centre: `pos`, `neg`, or `miss` (not recorded; treated as missing, never as negative) |
| `GLUL`, `ALDH2`, `ECHS1`, `SCP2` | Plasma protein levels, SomaScan v4.1 relative fluorescence units (RFU), ANML-normalized |

| Cohort | LG-IPMN | HG-IPMN | PDAC | CA19-9 recorded |
|---|---|---|---|---|
| Discovery | 24 | 14 | 18 | 48 |
| Validation | 46 | 9 | 12 | 60 |

## Outputs → manuscript

**Figure 5 — primary contrast (HG-IPMN + PDAC vs LG-IPMN)**

| Panel | Content | File(s) in `output/` |
|---|---|---|
| A | ROC curves, four proteins | `validation_Malignant_PDAC.svg`, `AUC_PDAC.xlsx` |
| B | Performance metrics at locked cut-offs | `metrics_PDAC.svg`, `metrics_PDAC.xlsx` |
| C | Confusion matrices at locked cut-offs | `cm_Malignant_PDACv.png` (single-row layout: `cm_Malignant_PDAC.png`) |
| D | Metrics, CA19-9 and CA19-9 + GLUL | `metrics_c19_GLUL_PDAC_cc.svg`, `metrics_CA19-9_GLUL_PDAC_cc.xlsx` |
| E | Confusion matrices, CA19-9 and CA19-9 + GLUL | `cm_ca19_GLUL_PDAC_cc.png` |

**Supplementary Figure 5 — secondary contrast (HG-IPMN vs LG-IPMN)**

| Panel | Content | File(s) in `output/` |
|---|---|---|
| A | ROC curves, four proteins | `validation_Malignant_IPMN.svg`, `AUC_IPMN.xlsx` |
| B | Performance metrics at locked cut-offs | `metrics_IPMN.svg`, `metrics_IPMN.xlsx` |
| C | Confusion matrices at locked cut-offs | `cm_Malignant_IPMNv.png` (single-row layout: `cm_Malignant_IPMN.png`) |
| D | Metrics, CA19-9 and CA19-9 + GLUL | `metrics_ca19_GLUL_IPMN_cc.svg`, `metrics_CA19-9_GLUL_IPMN_cc.xlsx` |
| E | Confusion matrices, CA19-9 and CA19-9 + GLUL | `cm_ca19_GLUL_IPMN_cc.png` |

**Supplementary Table 6 — discovery cut-offs:** `cutoffs_PDAC.xlsx`, `cutoffs_IPMN.xlsx`

The `.xlsx` metric tables also report the underlying TP/FN/TN/FP counts.
`analysis.html` is the knitted notebook with every table and plot, viewable
in a browser without running R.

## Reproducing the analysis

Requires R (≥ 4.4) with:

```r
install.packages(c("readxl", "dplyr", "ggplot2", "patchwork", "pROC",
                   "cutpointr", "cvms", "writexl", "scales", "rmarkdown"))
```

Then, from the repository root:

```r
rmarkdown::render("analysis.Rmd")
```

All bootstrap procedures use `set.seed(42)`; outputs are deterministic for a
given package version. The deposited outputs were generated with the versions
listed in `session_info.txt`.

## License

MIT — see `LICENSE`.

## Contact

Luca Mastrantoni (code and analysis); Carmine Carbone (corresponding author).
