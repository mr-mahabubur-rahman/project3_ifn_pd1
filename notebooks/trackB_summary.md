# Track B summary — Boukhaled et al. 2022 (Nat Immunol), scRNA-seq reanalysis (GSE199994)

## Data and pipeline
- Public data: gene expression only from 10x Multiome (no ATAC); 10 PBMC samples:
  HD1–2, P1–P4 (high IRC), P5–P8 (low IRC; groups from paper Fig 5c / Ext Data 7a).
- QC (paper thresholds) -> 97,142 cells; DoubletFinder -> 90,502; RPCA integration
  (paper: CCA), resolution 0.3 -> 15 clusters; SingleR (BlueprintEncode) + markers.
  Low-quality cluster (2,721 cells; ~1/3 of normal RNA content) excluded.
- CD4 T cells (24,455) re-integrated; resolution 0.3 -> 8 sub-clusters (paper: 8).
  Excluded CD8 contamination (899) and a CD74-high cluster (175) -> 23,381 cells.
- CD4 Teff (patients): high IRC 3,278 / low IRC 2,412 cells (paper 2,305 / 1,643; ratio 1.36 vs 1.40).

## Reproduction of paper findings
| Panel | Finding | Our result | Patient-level (4 vs 4) |
|---|---|---|---|
| 5a–b | CD4 subsets and marker genes | 22/22 named genes match; TEMRA not recovered | — |
| ED 7d | No baseline difference in 6 IRC ISGs (MX1 only) | MX1 only, P = 0.0017 | No differences |
| ED 7c | Hallmark IFN-α genes mixed | 9/9 named genes agree; 15 up in low, 18 up in high | — |
| 5h, 5k | NFAT, FLI1, RUNX1, TOX, DNMT3A up in high; FOS, JUN up in low | 9/10 agree (EZH2 not) | Same direction; NFATC2, NFAT5 fully separated |
| 5e | PD1 ligation enriched in low IRC CD4 Teff | NES 2.46, FDR 7.6e-9 (paper 2.19) | Robust to mito adjustment |
| ED 7h | PD1 ligation enriched in low IRC CD8 Teff | NES 2.28, FDR 5.6e-9 (paper 2.02) | — |
| 5i | High: GTPase/adhesion; low: OXPHOS/translation | Themes match | OXPHOS partly mito-linked |
| 5j | Chromatin-modifying enzymes up in high IRC | 23/23 genes same direction | Pathway result depends on ranking method |
| 5f | Regulons (CollecTRI proxy) | 10/15 agree; large regulons agree; MAF opposite | — |
| ED 7f | No cytokine/chemokine/cell-cycle differences | 0/159 genes | — |
| ED 7e | IDO1 similarly low in myeloid cells | ~1% of cells in both groups; patient-level P = 0.49 | — |
| ED 7g | Higher CD8 exhaustion in high IRC | Cell-level P = 1.2e-18 | Not significant (P = 0.11) |

Not possible or not reproduced: Fig 5c, Fig 6 and Ext Data 8 (no ATAC data); Fig 5g (IPA, commercial);
Fig 5d (custom gene sets unavailable; public proxies do not support Tfh enrichment in high IRC); EZH2.

## Robustness analyses (our additions)
1. **Pseudoreplication:** cell-level DE found 2,465/7,780 genes (32%); pseudobulk 328/10,744 (3%).
   Fold changes agree (Spearman 0.981). Robust at patient level: B2M, HLA-C, JUN, FOS (higher in low IRC).
   NFAT/exhaustion genes: same direction, underpowered with 4 vs 4.
2. **Sample-quality confounding:** low-IRC samples have higher mito % (Teff 8.1–11.0% vs 5.4–6.2%)
   and higher stress-gene scores; perfectly confounded with IRC group. Within-patient mito explains
   ~14% of the stress gap. After mito adjustment, OXPHOS enrichment drops 30–39% but stays significant;
   PD1 ligation, TNF/NF-κB, GTPase and adhesion themes unchanged or stronger.
3. **Ranking sensitivity:** chromatin organisation enriched in high IRC with patient-weighted ranking
   (P = 6e-9) but not with cell-pooled fold changes (FDR 0.31).
4. **Patient heterogeneity:** P1 drives much of the NFAT signal; P4 resembles low-IRC patients
   (consistent with paper Fig 5c); P5 is the weakest sample (fewest cells, monocyte loss).

## New observations
- Antigen-presentation program in low-IRC Teff: MHC-I (B2M, HLA-C), MHC-II (CD74), CIITA/RFX1 activity.
- MYC activity higher in low IRC, consistent with the translation/OXPHOS theme.

## Conclusion
Most of the paper's single-cell findings reproduce in direction. PD1-ligation enrichment (CD4 and CD8)
and MHC-I expression are the most robust. Most other claims rest on cell-level statistics and are
underpowered at the patient level; metabolic (OXPHOS) and AP-1 findings are partly confounded with
sample quality; the Tfh/MAF claim is the least supported.

Limitations: Seurat v5 + RPCA (paper v4 + CCA); single-nucleus RNA only; proxy gene sets and regulons;
4 vs 4 patients. Pending: full pySCENIC (100 runs) and a CCA rerun on the high-RAM machine.