# Differential Gene Expression Analysis: RNA-seq with PyDESeq2

## Overview
This project implements a complete RNA-seq differential gene expression pipeline in Python using PyDESeq2 — a Python reimplementation of the widely used R package DESeq2. The analysis identifies genes significantly affected by dexamethasone treatment in airway smooth muscle cells, then extends into pathway-level interpretation via Gene Set Enrichment Analysis (GSEA).

## Biological Question
Which genes — and which broader biological pathways — are altered in airway smooth muscle cells following treatment with dexamethasone, a corticosteroid commonly used to manage asthma?

## Data
- **Raw RNA-seq count matrix:** `airway_scaledcounts.csv`
- **Sample metadata:** `airway_metadata.csv`
- Source: [BRITE-REU programming workshops](https://github.com/BRITE-REU/programming-workshops)

Both files download automatically via `wget` when running the notebook.

## Methods & Tools

**Language:** Python 3.10+

| Library | Purpose |
|---|---|
| `pydeseq2` | Differential expression testing (negative binomial GLM) |
| `pandas` / `numpy` | Data manipulation |
| `scanpy` | PCA and sample clustering visualization |
| `gseapy` | Gene Set Enrichment Analysis against KEGG pathways |
| `sanbomics` | Gene ID-to-symbol annotation, volcano plot |
| `matplotlib` / `seaborn` | Heatmap visualization |

**Pipeline:**
1. Load raw counts and metadata; filter genes with zero counts across all samples
2. Run PyDESeq2 differential expression analysis (dexamethasone-treated vs. control)
3. Annotate results with human-readable gene symbols
4. Verify sample separation via PCA as a quality control step
5. Run GSEA against KEGG 2021 Human pathways using DESeq2 test statistics as the ranking metric
6. Visualize results with a volcano plot and a heatmap of the top 50 differentially expressed genes

## Key Results
- PyDESeq2 identifies a clear set of significantly differentially expressed genes (padj < 0.05, |log2FC| > 0.5) between treated and control samples
- PCA confirms treatment groups separate cleanly along the first principal component, supporting confidence in the differential expression calls
- GSEA reveals KEGG pathways enriched among differentially expressed genes, providing biological context beyond individual gene hits
- The volcano plot and heatmap together summarize both the breadth (volcano) and detail (heatmap) of the expression changes

## How to Run

**Option 1 — Google Colab**
1. Upload `Differential_Gene_Expression.ipynb` to Google Colab
2. Run all cells top to bottom — data downloads automatically, packages install via pip

**Option 2 — Local environment**
```bash
git clone https://github.com/yourusername/differential-gene-expression
cd differential-gene-expression
pip install -r requirements.txt
jupyter notebook Differential_Gene_Expression.ipynb
```

**Note:** PyDESeq2's internal API (e.g., accessing normalized counts) has changed slightly across versions. If the heatmap section throws an attribute error, check `dds.layers` keys against your installed `pydeseq2` version.

## Project Structure
```
differential-gene-expression/
├── README.md
├── Differential_Gene_Expression.ipynb
├── requirements.txt
└── data/       # Not tracked — datasets download automatically
```
