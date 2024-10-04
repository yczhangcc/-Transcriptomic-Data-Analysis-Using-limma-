# 🐸 Transcriptomic Data Analysis Using limma 🧬

## 🌟 Project Overview
Welcome to the **Egg Pigmentation Investigation**! 🐸🔬 This project is all about uncovering the secrets behind **gene expression** in **glass frogs** (*Espadarana prosoblepon*) 🐸. Using RNA-seq data, we aim to identify which genes are responsible for **pigmented** vs **unpigmented** eggs.

## 🗂 Project Structure

```bash
/project-directory
  ├── 📁 data/                          
  ├── 📁 scripts/                       
  ├── 📁 results/                       
  ├── 📄 README.md                      
  ├── 📊 data/your_count_data.csv       # Gene expression count matrix
  ├── 📝 data/your_sample_info.csv      # Sample metadata (grouping info)
  ├── 📜 analysis.Rmd                   # Full analysis script
```

## 💻 Requirements

- **R Version**: 4.4 or later
- **R Packages**:
  - `limma` 📊
  - `edgeR` 🧬
  - `ggplot2` 📈

## 🔧 Installation

To get started with **limma** and friends, run this code in R:

```r
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("limma")
```

## 📄 Data Description
- **your_count_data.csv**: Contains all our precious RNA-seq count data, where each row represents a gene 🧬 and each column is a sample.
- **your_sample_info.csv**: Metadata file describing the samples (e.g., **Pigmented** 🟡 vs. **Unpigmented** ⚪).

## 🔍 Analysis Pipeline

1. **📥 Data Loading**:
   - Load RNA-seq data and sample information.
   
2. **🧼 Data Preprocessing**:
   - Clean up the data by filtering out lowly expressed genes using `filterByExpr()` 🧹.
   - Normalize with `calcNormFactors()` to get everything on the same scale ⚖️.

3. **📊 Linear Model Fitting**:
   - Fit a **linear model** with `lmFit()` to analyze gene expression.

4. **🧮 Differential Expression**:
   - Identify differentially expressed genes between conditions using contrast matrices and empirical Bayes moderation 🔥.

5. **📊 Results**:
   - Use `topTable()` to extract the top hits and create a **volcano plot** 🌋 to visualize the differences.

## 💡 Example Code

```r
library(limma)
library(edgeR)

# Load count data
counts <- read.csv("data/your_count_data.csv", row.names = 1)
sample_info <- read.csv("data/your_sample_info.csv")

# Create DGEList object
dge <- DGEList(counts = counts, group = sample_info$group)

# Filter, normalize, and create design matrix
keep <- filterByExpr(dge)
dge <- dge[keep, , keep.lib.sizes = FALSE]
dge <- calcNormFactors(dge)
design <- model.matrix(~0 + group, data = sample_info)

# Fit the linear model and apply empirical Bayes moderation
fit <- lmFit(dge, design)
fit2 <- eBayes(fit)

# Test contrasts and get results
contrast_matrix <- makeContrasts(groupA - groupB, levels = design)
fit2 <- contrasts.fit(fit, contrast_matrix)
fit2 <- eBayes(fit2)

# Get top results and generate volcano plot
results <- topTable(fit2, adjust = "BH")
volcanoplot(fit2, highlight = 10)
```

## 🎨 Visuals and Outputs
- **differential_expression_results.csv**: A table of the **top differentially expressed genes** 🧬.
- **Volcano plot** 🌋: A fancy plot showing the biggest gene expression differences between groups!
