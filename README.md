# Reproduction of Boukhaled et al. 2022 (*Nature Immunology*)

**"Pre-encoded responsiveness to type I interferon in the peripheral immune system defines outcome of PD1 blockade therapy"**
Boukhaled GM *et al.*, *Nat Immunol* 23:1273–1283 (2022). https://doi.org/10.1038/s41590-022-01262-7

This repository reproduces and critically reanalyses the paper using its public data, in two tracks:

- **Track A — clinical and statistical results** (Figs 1–4, Extended Data 1–6) from the published source data.
- **Track B — single-cell transcriptomics** (Fig 5, Extended Data 7) from the gene-expression part of the 10x Multiome data (GEO GSE199994).

## Key findings

- **Every panel with source data reproduces numerically**, and the single-cell results largely reproduce in direction
  (e.g. 22/22 CD4 subset marker genes; PD1-ligation gene set enriched in low-IRC Teff, NES 2.46 in CD4 and 2.28 in CD8).
- **The headline association holds, but is weaker than reported.** Higher pre-therapy CD4 Teff IRC predicts shorter survival
  as a continuous variable (HR 1.40 per 0.1 IRC, P = 0.012), but the paper's optimal cut-point inflates the effect
  (HR 3.60; search-corrected P = 0.032 vs naive 0.0024). The risk model does separate survival in the independent NSCLC cohort.
- **Several supporting claims do not survive appropriate analysis:** IS "connectivity" (Fig 1f; permutation P = 0.31 vs 1.2e-5),
  on-treatment IRC change (Fig 3a; consistent with regression to the mean), and the baseline-stability test
  (Ext Data 1c; reported P reproduced only with an equivalence margin of ±1.0).
- **Single-cell caveats:** most differences are significant only cell-by-cell (32% of genes) and far fewer at the patient level
  (3%, pseudobulk 4 vs 4); low-IRC samples have lower quality (higher mitochondrial %), which partly explains the OXPHOS signal.

Full details: [`notebooks/trackA_summary.md`](notebooks/trackA_summary.md) and [`notebooks/trackB_summary.md`](notebooks/trackB_summary.md).

## Repository structure

```
notebooks/        Jupyter notebooks (R kernel), run in order, plus setup log and summaries
figures/trackA/   Clinical/statistical figures
figures/trackB/   Single-cell figures
results/tables/   Result tables (CSV)
metadata/         Sample sheet (P1–P4 high IRC, P5–P8 low IRC, HD1–2 healthy donors)
environment.yml   Conda environment (R 4.4, Seurat 5.5.1, and all packages)
```

| Notebook | Content |
|---|---|
| `00_setup_log.md` | Data inspection, environment setup |
| `01_load_qc` | Loading 10 samples, QC (paper thresholds) |
| `02_doublets` | DoubletFinder per sample |
| `03_integration` | Normalisation, RPCA integration, clustering |
| `04_annotation` | SingleR + marker-based cell type annotation |
| `05_cd4_subclustering` | CD4 T cell sub-clustering (Fig 5a) |
| `06_cd4_teff_analysis` | Fig 5b, 5d–e, 5h–k; Ext Data 7c–d, 7f–h; pseudobulk, mito adjustment |
| `07_tf_activity` | TF activity with decoupleR/CollecTRI (stand-in for Fig 5f) |
| `08_trackA_clinical` | All source-data analyses, cut-point, regression-to-mean and permutation checks |
| `09_extfig7e_ido1` | Ext Data 7e: IDO1 in myeloid cells |

## Data (not included in this repository)

- **scRNA-seq:** GEO [GSE199994](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE199994), filtered gene-expression matrices,
  placed as `data/<sample>/{barcodes,features,matrix}.tsv.gz` for HD1, HD2, P1–P8.
- **Source data:** Supplementary files MOESM3–MOESM10 from the paper, placed in `source_data/`.

## How to reproduce

```bash
conda env create -f environment.yml
conda activate ifn
Rscript -e 'remotes::install_github("chris-mcginnis-ucsf/DoubletFinder", upgrade = "never")'
jupyter lab
```

Set `proj` at the top of each notebook to your project path, then run the notebooks in order.
Notebooks 01–07 need about 12 GB RAM; notebook 08 runs on any machine.

## Deviations from the paper

Seurat v5 with RPCA integration (paper: v4 with CCA); single-nucleus RNA only (no ATAC: Fig 6, Ext Data 8 and
Fig 5c not reproducible); Wilcoxon (presto) instead of single-cell DESeq2; decoupleR/CollecTRI instead of SCENIC;
proxy gene sets where the paper's custom sets are unavailable. CyTOF raw data are not public.
Planned: full pySCENIC (100 runs) and a CCA re-run on a high-memory machine.

## Author

Mahabubur Rahman — ORCID [0000-0002-5535-5816](https://orcid.org/0000-0002-5535-5816)

All credit for the original study, data and design belongs to Boukhaled *et al.* This is an independent reanalysis.