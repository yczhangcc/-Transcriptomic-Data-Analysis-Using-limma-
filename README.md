# 🐸 **Glassfrog Transcriptomic Data Analysis Project** 🧬
### **README**

---

### **Project Overview**
Welcome to the *Glassfrog Transcriptomics Project*! In this project, we are studying the gene expression differences between *pigmented* and *unpigmented* eggs in glassfrogs using RNA-seq data. Our goal is to understand how pigmentation is controlled at the molecular level. We use the powerful **Limma-Voom** pipeline to detect differentially expressed genes (DEGs) between the two groups. Let's dive into the world of gene expression! 🎉

---

### 🧾 **Data Overview**
- **Input Data:**
  - `final_subset_grouped.xlsx`: Contains RNA-seq raw counts for different frog samples.
  - `meta_glassfrog.xlsx`: Metadata that includes the pigmentation group (pigmented vs unpigmented) and batch effect information.
  
- **Output Data:**
  - `voom_differential_expression_results.csv`: Differentially expressed genes after applying the Limma-Voom method.
  - `differential_expression_results_pigmented_vs_unpigmented.csv`: Comparison results between pigmented and unpigmented groups.
  - `tpms_output.xlsx`: Transcripts Per Million (TPM) data for gene expression quantification.

---

### **Workflow Overview**
Here is a step-by-step breakdown of our analysis workflow:

---

### **1. Data Preprocessing**
First, we normalize the raw RNA-seq counts using **TMM normalization** (Trimmed Mean of M-values) to account for differences in sequencing depth. 🧪  
Next, we filter out low-expressed genes (those with counts below 1 in most samples), ensuring our analysis focuses on the most relevant genes.

---

### **2. Differential Expression Analysis with Limma-Voom**
The **Limma** package in R is a widely-used tool for differential expression analysis, originally designed for microarray data but adapted for RNA-seq with **Voom**. Here’s how the analysis works:
- **Design Matrix Construction:** We construct a design matrix that incorporates two factors:
  - **Pigmentation status** (`pigmented` vs `unpigmented`).
  - **Batch effect** (to adjust for batch-to-batch variability).
  
- **Voom Transformation:** We apply **voom** to the count data, which transforms it into log2 counts per million (CPM) and estimates the mean-variance relationship. This makes the data suitable for linear modeling while stabilizing variance across the expression levels.

---

### **3. Fitting the Linear Model**
We fit a linear model using **lmFit()**, which allows us to assess the impact of the pigmentation condition while adjusting for batch effects.

---

### **4. Empirical Bayes Moderation**
Limma employs **empirical Bayes moderation**, which helps shrink the estimated standard errors of the gene-wise log-fold changes toward a common value. This enhances the statistical power of our analysis. 🔬

---

### **5. Identifying Differentially Expressed Genes (DEGs)**
Using the **topTable()** function, we extract the list of DEGs with the following key statistics:
- **logFC:** Log fold change between pigmented and unpigmented groups.
- **P.Value & adj.P.Val:** Raw and adjusted p-values, respectively. We control for multiple testing using the **False Discovery Rate (FDR)** to ensure a low rate of false positives (genes falsely detected as differentially expressed). 🧠
- **AveExpr:** Average expression level across samples.
  
We set an FDR cutoff of 0.05 to identify statistically significant DEGs.

---

### **Statistical Tests Summary** 🧮
- **Linear Modeling:** We use linear models to estimate the effect of pigmentation while adjusting for batch.
- **Empirical Bayes:** Shrinks the variance of gene-wise statistics to reduce the noise in our data.
- **FDR Adjustment:** Ensures that the proportion of false positives is kept under control when interpreting the results.

---

### **Key R Scripts**
- **voom.Rmd:** Contains the complete R code for performing the Limma-Voom analysis, including data normalization, Voom transformation, linear modeling, and extraction of DEGs.
- **limma_differential_expression_results.csv:** Full results of the differential expression analysis between the pigmentation groups.
  
---

### **Interesting Results**
🧬 From the analysis, we discovered a number of genes that exhibit significant differences in expression between pigmented and unpigmented eggs. These genes could potentially be involved in the pigmentation process, providing insights into the molecular mechanisms that underlie egg pigmentation in glassfrogs! 🌿

---

### **Directory Structure**
```
.
├── final_subset_grouped.xlsx        # Raw RNA-seq count data
├── meta_glassfrog.xlsx              # Metadata file with pigmentation and batch information
├── voom.Rmd                         # R script for differential expression analysis
├── differential_expression_results_pigmented_vs_unpigmented.csv   # DEGs for pigmentation comparison
├── tpms_output.xlsx                 # Transcripts Per Million (TPM) values for gene expression quantification
└── limma_differential_expression_results.csv    # General differential expression results
```

---

### **Fun Visualization** 🎨
✨ Check out the **mean-variance plots** generated by the **Voom transformation** in `voom.Rmd`. These plots demonstrate how the variance in gene expression data is stabilized after Voom, enabling more reliable statistical testing.

---

### **Contact Information**
For any questions or clarifications, feel free to reach out to the project lead.

🐸🧬🌿 **Happy exploring the world of glassfrog transcriptomics!** 🌿🧬🐸
