# 🧬 Airway RNA-seq Analysis: Dexamethasone Treatment in Human Airway Cells

[![R](https://img.shields.io/badge/R-4.0+-blue.svg)](https://www.r-project.org/)
[![DESeq2](https://img.shields.io/badge/DESeq2-1.36+-green.svg)](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Overview

This project analyzes RNA-seq data from human airway smooth muscle cells treated with **dexamethasone** – a common anti-inflammatory drug used to treat asthma. The goal is to identify which genes are up-regulated or down-regulated in response to treatment.

**Key Question:** How does dexamethasone change gene expression patterns in airway cells?

## 🔬 Experimental Design

| Factor | Details |
|--------|---------|
| **Samples** | 8 total (4 treated, 4 untreated) |
| **Treatment** | Dexamethasone (1 μM, 18 hours) |
| **Cell Type** | Human airway smooth muscle cells |
| **Donors** | 4 different individuals (biological replicates) |
| **Sequencing** | Illumina HiSeq 2000 |

## 📊 Results Summary

- **Total genes analyzed:** ~20,000
- **Significantly differentially expressed genes (padj < 0.05, |log2FC| > 1):** **1138 genes**
- **Up-regulated genes (log2FC > 1):** **XXX** *(fill in after running code below)*
- **Down-regulated genes (log2FC < -1):** **XXX** *(fill in after running code below)*

### Top 10 Most Significant Genes

| Gene Symbol | Log2 Fold Change | Adjusted P-value | Regulation |
|-------------|------------------|------------------|------------|
| SPARCL1 | 4.57 | 3.58e-132 | Up ⬆️ |
| CACNB2 | 3.29 | 8.60e-132 | Up ⬆️ |
| DUSP1 | 2.95 | 1.22e-124 | Up ⬆️ |
| SAMHD1 | 3.76 | 1.88e-124 | Up ⬆️ |
| MAOA | 3.35 | 2.57e-119 | Up ⬆️ |
| GPX3 | 3.73 | 5.98e-107 | Up ⬆️ |
| STEAP2 | 1.97 | 1.96e-100 | Up ⬆️ |
| NEXN | 2.03 | 2.17e-96 | Up ⬆️ |
| MT2A | 2.21 | 1.50e-91 | Up ⬆️ |
| ADAMTS1 | 2.34 | 2.45e-85 | Up ⬆️ |

> **Biological insight:** SPARCL1 (SPARC-like protein 1) is strongly up-regulated by dexamethasone, consistent with its role in airway remodeling and inflammation regulation. VCAM1 (log2FC = -3.69) is among the top down-regulated genes, suggesting reduced inflammatory adhesion molecule expression.

## 📈 Visualizations

### Volcano Plot
![Volcano Plot](Volcano%20Plot%20treated%20vs%20control.png)

*Genes with |log2FC| > 1 and padj < 0.05 are highlighted in red (up) and blue (down)*

### MA Plot
![MA Plot](fold%20change%20scatter%20plot.png)

*Shows log2 fold change vs mean normalized expression. Blue points = statistically significant genes.*

### Top 50 Genes Heatmap
![Heatmap](top%2050%20significant%20genes%20heatmap.png)

*Hierarchical clustering of the 50 most significant genes across all 8 samples*

## 🛠️ Methods

### Analysis Pipeline

### Statistical Thresholds

| Parameter | Value |
|-----------|-------|
| Significance cutoff (padj) | < 0.05 |
| Fold change cutoff (log2FC) | > |1| (2x change) |
| Multiple testing correction | Benjamini-Hochberg (FDR) |

### Software & Packages

| Tool | Version | Purpose |
|------|---------|---------|
| R | 4.0+ | Analysis environment |
| DESeq2 | 1.36+ | Differential expression |
| ggplot2 | 3.4+ | Visualization |
| pheatmap | 1.0+ | Heatmap generation |
| ggrepel | 0.9+ | Label optimization |
| org.Hs.eg.db | 3.16+ | Gene ID mapping |

## 📁 Repository Contents

## 🚀 How to Reproduce This Analysis

### Option 1: Load the Workspace (Quickest)

```r
# Load the complete R workspace
load("airway_deseq2_analysis.RData")

# View significant genes
head(sig_genes[, c("symbol", "log2FoldChange", "padj")])

# Count up vs down regulated
table(sig_genes$log2FoldChange > 0)
# 1. Install required packages
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install(c("DESeq2", "airway", "ggplot2", "pheatmap", "ggrepel", "org.Hs.eg.db"))

# 2. Load libraries and data
library(DESeq2)
library(airway)
data("airway")

# 3. Create DESeq2 object
dds <- DESeqDataSet(airway, design = ~ cell + dex)

# 4. Filter low-expressed genes
keep <- rowSums(counts(dds) >= 10) >= 3
dds <- dds[keep, ]

# 5. Run DESeq2
dds <- DESeq(dds)

# 6. Extract results
res <- results(dds, contrast = c("dex", "trt", "untrt"))

# 7. Map gene symbols
library(org.Hs.eg.db)
res_df <- as.data.frame(res)
res_df$symbol <- mapIds(org.Hs.eg.db, 
                        keys = rownames(res_df), 
                        keytype = "ENSEMBL", 
                        column = "SYMBOL",
                        multiVals = "first")

# 8. Filter significant genes
sig_genes <- res_df[which(res_df$padj < 0.05 & abs(res_df$log2FoldChange) > 1), ]
sig_genes <- sig_genes[order(sig_genes$padj), ]

# 9. View results
head(sig_genes[, c("symbol", "log2FoldChange", "padj")])
)
📊 Key Findings
Biological Interpretation
Dexamethasone is a synthetic glucocorticoid widely used to control asthma symptoms. Our analysis reveals:

Up-regulated genes (like SPARCL1, DUSP1, SAMHD1) are involved in:

Anti-inflammatory signaling pathways

Extracellular matrix organization

Cellular stress responses

Down-regulated genes (like VCAM1, KCTD12, ADAM12) are involved in:

Inflammatory adhesion molecules (VCAM1)

Tissue remodeling processes

Smooth muscle cell proliferation

Comparison with Published Literature
These results align with published studies showing that glucocorticoids:

Suppress pro-inflammatory genes (VCAM1, CXCL12)

Activate anti-inflammatory programs (DUSP1, TSC22D3)

Regulate genes involved in airway remodeling (SPARCL1, ADAMTS1)

📚 Citation
If you use this analysis or dataset, please cite:

Dataset:
Himes, B.E., et al. (2014). "RNA-Seq transcriptome profiling of primary human airway smooth muscle cells." Scientific Data 1, 140031.

Methods:
Love, M.I., Huber, W., Anders, S. (2014). "Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2." Genome Biology 15(12), 550.

This analysis:
Adeeba, GitHub: adeeba9-ux

📞 Contact
For questions about this analysis:

GitHub Issues: Open an issue in this repository

Email: adeebleads9@gmail.com

📜 License
This project is licensed under the MIT License - see the LICENSE file for details.

⭐ Star this repo if you found it useful!

---

## 📝 Quick Fix for the Numbers

**Run this in R to get your exact up/down counts and fill them in:**

```r
# Count up vs down regulated
table(sig_genes$log2FoldChange > 0)

# Replace "XXX" in the README with these numbers
