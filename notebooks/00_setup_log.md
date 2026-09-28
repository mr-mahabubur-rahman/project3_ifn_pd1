# 00 — Project setup log

## Step 1 — Inspect data
- GEO GSE199994, obtained pre-organised as data/HD1, HD2, P1–P8
- Each sample: barcodes.tsv.gz, features.tsv.gz, matrix.mtx.gz (filtered cells)

## Step 2 — Check feature types and cell counts
- 36,601 features, all "Gene Expression" -> RNA only, no ATAC peaks or fragments
- Cells: HD1 7,565; HD2 11,952; P1 10,845; P2 14,493; P3 13,010; P4 10,787;
  P5 2,992; P6 9,948; P7 9,704; P8 9,724 (total ~101,000)
- Scope: Fig 5 + Ext Data 7 reproducible; Fig 6, Ext Data 8, Fig 5c (SNF) not possible

## Step 3 — Project structure and sample sheet
- Folders: data/ (read-only), metadata/, notebooks/, results/{objects,tables},
  figures/{trackA,trackB}, source_data/
- metadata/sample_sheet.csv: P1–P4 high_IRC, P5–P8 low_IRC, HD1–2 healthy
  (groups from paper Fig 5c and Ext Data Fig 7a)

## Step 4 — Environment
- WSL2 memory raised via C:\Users\Lenovo\.wslconfig (memory=13GB, swap=16GB)
- conda env "ifn": R 4.4, Seurat 5.5.1, DoubletFinder 2.0.6 (GitHub), SingleR 2.8.0,
  celldex 1.16.0, DESeq2 1.46.0, fgsea 1.32.2, msigdbr 26.1.1, survival 3.8.12,
  survminer 0.5.2, tidyverse 2.0.0
- Note: paper used Seurat 4.1; v5 differs in integration functions

## Step 5 — Launch JupyterLab
- conda activate ifn; cd /mnt/d/project3_IFN-I; jupyter lab --no-browser

## Session end — notebook 03 in progress
- Notebooks 01 (QC) and 02 (doublets) complete; 90,502 singlets saved.
- Notebook 03: WSL crashed during Step 9.3 (likely out of memory).
- Next: rerun 03 with Step 9.2b checkpoint and memory-safe Step 9.3; monitor with `watch -n 5 free -h`.
