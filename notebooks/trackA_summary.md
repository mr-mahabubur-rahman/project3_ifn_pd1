# Track A summary — Boukhaled et al. 2022 (Nat Immunol), clinical and statistical reproduction

## Data
- Source data MOESM3–10 (21 sheets). Discovery (INSPIRE, n = 31: HNSCC 19, melanoma 12),
  melanoma validation (n = 28), NSCLC validation (n = 34). R survival, survminer, maxstat.
- OS given to 0.1 month; PFS not provided; validation-cohort IRC given only as high/low labels.

## Reproduction of paper results
| Panel | Paper | Ours | Verdict |
|---|---|---|---|
| 1e | HD > NCB: B, NK, pDC (P = 0.014, 0.021, 0.021) | 0.0141, 0.0205, 0.0205 | Exact |
| 1f–g | Connectivity differences (Wilcoxon) | Same P values; 1g P = 0.023 | Reproduced (see 4 below) |
| 2d | P = 2.4e-8 | 2.36e-8 | Exact (circular, see 2) |
| 2e–f | Log-rank 0.0025 / 0.0031 / 0.019 | 0.0024 / 0.0029 / 0.019 | Reproduced (OS rounding) |
| 3a–c | 0.002, 0.04; 3b ns; 3c 0.016 | 0.00195, 0.0395; 0.64; 0.0156 | Reproduced |
| 4c–e | PD-L1 0.014, IL-10 0.037; R 0.71 / 0.39 / 0.49 | 0.0139, 0.0366; identical R | Exact |
| 4f, 4h | 0.0054; 4.5e-5 | 0.0053; 4.4e-5 | Reproduced |
| 4h text | 81.5%, 70.2%, 77.8%, 65.3% | 81.6%, 70.2%, 77.8%, 65.4% | Reproduced |
| ED 1c | TOST P = 7.9e-48 | Reproduced only with margin ±1.0 | Uninformative (see 5) |
| ED 1d–e | ns; P = 0.11 (21/10) | 0.31; 0.11 (21/10) | Reproduced |
| ED 2b | No subset IS associated with OS | Continuous P >= 0.33 for all 11 | Reproduced |
| ED 3e | Kruskal–Wallis P = 4.8e-48 | 4.78e-48 (Friedman 1.6e-50) | Exact |
| ED 4a–b, 4e | ns; ns; P = 0.014 (20/11) | ns; 0.31; 0.0136 | Reproduced |
| ED 5a–b | ns; 0.062 | 0.70; 0.0625 | Reproduced |
| ED 6a | P = 0.25 (CD14+ PD-L1) | 0.34 (total myeloid only) | Approximately |
| ED 6c, 6e | 0.0065; 0.00037 | 0.0066; 0.00037 | Reproduced |

Not reproducible: Fig 4g, ED 6b, 6d (model inputs not provided); PFS panels; ED 4g; all CyTOF-derived panels.

## Critical re-analysis (our additions)
1. **Cut-point selection.** Discovery CD4 Teff IRC split = the surv_cutpoint optimum (0.66). Naive P = 0.0024;
   corrected for the search P = 0.032; pre-specified median split HR 1.66 (0.70–3.94), P = 0.25;
   continuous HR 1.40 per 0.1 IRC (1.07–1.81), P = 0.012. Association real, but HR inflated (3.6 vs ~1.8/SD).
   CD8 Teff IRC: corrected P = 0.046, continuous P = 0.049 (borderline). Validation cut-offs possibly
   re-optimised per cohort (35% / 43% / 56% high); cannot check.
2. **Circularity (Fig 2d).** P = 2.36e-8 is the minimum possible for non-overlapping groups defined by the same variable.
3. **Regression to the mean (Fig 3a).** Oldham r = -0.11 (P = 0.57); SD 0.180 -> 0.162. On-treatment IRC change
   fully consistent with regression to the mean; biological interpretation not supported over this null.
4. **Non-independence (Fig 1f).** Permuting patients: P = 0.31 vs Wilcoxon on 55 correlations P = 1.2e-5.
   Connectivity difference not supported; Fig 1g likely similarly inflated (untestable).
5. **Equivalence margin (ED 1c).** Paper's P reproduced exactly with margin ±1.0 (> 2x mean IS). Real agreement:
   r = 0.89, limits of agreement ~±14% of mean, small drift (P = 0.007). No test–retest data for IRC itself.
6. **Multiple testing and dichotomisation.** Fig 1e HD differences do not survive BH; pDC IS "significant" only
   with optimal cut (P = 0.019 vs continuous 0.35); total myeloid IDO continuous HR 0.85/SD, P = 0.29.
7. **Model validation.** Test set (NSCLC): HR 6.58 (2.04–21.2), 26/30 correctly classified at 2 years.
   Text percentages are sensitivity-type rates pooled over training + test (optimistic).
8. **Fig 3c** has no comparison group: all 7 paired biopsies increased TuIS regardless of IRC group.

## Source-data issues
- Fig 1f–g header says "Pearson" but values are exact Spearman coefficients.
- Fig 1f P-value brackets appear swapped relative to the underlying comparisons.
- ED 5b: 2 high-IRC pairs in source data vs 4 in legend. ED 3e: Kruskal–Wallis used for repeated measures.

## Conclusion
Every panel with source data reproduces numerically. The headline association (higher pre-therapy CD4 Teff IRC,
shorter survival) survives correction for cut-point selection and holds as a continuous variable, and the risk
model separates survival in an independent NSCLC cohort. However, effect sizes are inflated by optimal
cut-points, the validation cut-off procedure is unclear, and several supporting claims (Fig 1f connectivity,
Fig 3a on-treatment change, ED 1c stability P value, IDO) are not supported or cannot be independently confirmed.
Recommended: report continuous associations, apply the discovery cut-off unchanged to validation cohorts,
report IRC test–retest reliability, and use patient-level inference.