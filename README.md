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
- **Significantly differentially expressed genes (padj < 0.05, |log2FC| > 1):** 1138 genes

Run this in R to get up/down counts:
```r
table(sig_genes$log2FoldChange > 0)
Top 10 Most Significant Genes
Gene Symbol	Log2 Fold Change	Adjusted P-value	Regulation
SPARCL1	4.57	3.58e-132	Up ⬆️
CACNB2	3.29	8.60e-132	Up ⬆️
DUSP1	2.95	1.22e-124	Up ⬆️
SAMHD1	3.76	1.88e-124	Up ⬆️
MAOA	3.35	2.57e-119	Up ⬆️
GPX3	3.73	5.98e-107	Up ⬆️
STEAP2	1.97	1.96e-100	Up ⬆️
NEXN	2.03	2.17e-96	Up ⬆️
MT2A	2.21	1.50e-91	Up ⬆️
ADAMTS1	2.34	2.45e-85	Up ⬆️
Biological insight: SPARCL1 is strongly up-regulated by dexamethasone. VCAM1 (log2FC = -3.69) is among the top down-regulated genes.

📈 Visualizations
Volcano Plot
https://Volcano%2520Plot%2520treated%2520vs%2520control.png

MA Plot
https://fold%2520change%2520scatter%2520plot.png

Top 50 Genes Heatmap
https://top%252050%2520significant%2520genes%2520heatmap.png

🛠️ Methods
Statistical Thresholds
Parameter	Value
Significance cutoff (padj)	< 0.05
Fold change cutoff (log2FC)	>	1	(2x change)
Multiple testing correction	Benjamini-Hochberg (FDR)
Software & Packages
Tool	Version	Purpose
R	4.0+	Analysis environment
DESeq2	1.36+	Differential expression
ggplot2	3.4+	Visualization
pheatmap	1.0+	Heatmap generation
org.Hs.eg.db	3.16+	Gene ID mapping
📁 Repository Contents
airway_deseq2_analysis.RData - Complete R workspace

significant_genes.csv - All significant genes

significant_genes.xlsx - Excel version

Volcano Plot treated vs control.png - Volcano plot

top 50 significant genes heatmap.png - Heatmap

fold change scatter plot.png - MA plot

🚀 How to Reproduce
Load Workspace (Quickest)
r
load("airway_deseq2_analysis.RData")
head(sig_genes[, c("symbol", "log2FoldChange", "padj")])
table(sig_genes$log2FoldChange > 0)
Run Full Analysis
r
# Install packages
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install(c("DESeq2", "airway", "ggplot2", "pheatmap", "ggrepel", "org.Hs.eg.db"))

# Load and run DESeq2
library(DESeq2)
library(airway)
data("airway")
dds <- DESeqDataSet(airway, design = ~ cell + dex)
keep <- rowSums(counts(dds) >= 10) >= 3
dds <- dds[keep, ]
dds <- DESeq(dds)
res <- results(dds, contrast = c("dex", "trt", "untrt"))

# Map symbols
library(org.Hs.eg.db)
res_df <- as.data.frame(res)
res_df$symbol <- mapIds(org.Hs.eg.db, keys = rownames(res_df), 
                        keytype = "ENSEMBL", column = "SYMBOL", multiVals = "first")

# Get significant genes
sig_genes <- res_df[which(res_df$padj < 0.05 & abs(res_df$log2FoldChange) > 1), ]
sig_genes <- sig_genes[order(sig_genes$padj), ]
head(sig_genes[, c("symbol", "log2FoldChange", "padj")])
📊 Key Findings
Up-regulated genes (SPARCL1, DUSP1, SAMHD1) are involved in:

Anti-inflammatory signaling pathways

Extracellular matrix organization

Down-regulated genes (VCAM1, KCTD12, ADAM12) are involved in:

Inflammatory adhesion molecules

Tissue remodeling processes

📚 Citation
Dataset: Himes, B.E., et al. (2014). Scientific Data 1, 140031.

Methods: Love, M.I., Huber, W., Anders, S. (2014). Genome Biology 15(12), 550.

This analysis: adeeba9-ux

📞 Contact
Email: adeebleads9@gmail.com

📜 License
MIT License - see LICENSE file.

Last updated: May 2026
