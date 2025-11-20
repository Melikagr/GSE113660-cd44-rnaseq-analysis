# 🧬 Transcriptomic Analysis of CD44_high vs CD44_low Subpopulations (GSE113660)
*A complete, reproducible RNA-seq pipeline integrating preprocessing, differential expression analysis, visualization, and machine learning — combined with a personal learning journey in bioinformatics.*

---

## 👋 About This Project

This repository captures both a **technical analysis** and a **personal milestone** in my transition from a wet-lab–oriented background into **bioinformatics**, **computational biology**, and **biomedical data science**.

I built the entire workflow independently — from raw GEO HTSeq counts through DESeq2 modeling, PCA, heatmapping, and machine-learning classification.  
This is my first true end-to-end RNA-seq project, and it represents the point where computational methods finally “clicked” for me as tools for uncovering meaningful biology.

---

## 🎯 Why CD44 and Why This Dataset?

**CD44** is a biologically rich and well-studied surface marker involved in:

- cancer stemness  
- adhesion and migration  
- tumor heterogeneity  
- therapy resistance  

CD44_high and CD44_low populations often have striking transcriptomic differences.  
Dataset **GSE113660** was ideal for a first full pipeline because it provides:

- FACS-sorted **CD44_high**, **CD44_low**, and **unsorted** subpopulations  
- clean and clearly defined experimental groups  
- a manageable sample size  
- a meaningful biological question:  

> **Can gene expression profiles reliably distinguish cancer cell subpopulations defined by CD44 expression levels?**

This single question shaped the entire analysis.

---

## 🧠 Learning Goals

I designed this project to challenge myself across the full RNA-seq analysis spectrum.  
My aims included:

- Understanding count-matrix preprocessing deeply  
- Moving beyond “tutorial-level” DESeq2 and actually learning the underlying logic  
- Producing clean, interpretable visualizations (PCA, clustermaps)  
- Training and validating ML models on high-dimensional biological data  
- Practicing reproducibility (conda environments, version control, structured repo)  
- Connecting my wet-lab genetics background with computational methods  

By the end, I knew this was the direction I want to pursue in graduate school.

---

## 🌱 What This Project Taught Me

Along the way, I learned:

- how preprocessing decisions propagate into downstream statistics  
- the importance of variance stabilization and FDR corrections  
- how ML behaves in small-n, large-p biological datasets  
- how to structure a reproducible pipeline that others can run  
- that computational and biological reasoning reinforce each other  

This project strengthened both my technical confidence and my motivation to continue studying bioinformatics.

---

# 📚 Dataset Description

**GSE113660** — Human *alveolar rhabdomyosarcoma* Rh41 cell line, sorted by CD44 expression  
🔗 https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE113660

### Experimental groups:

| Phenotype    | GEO Suffix |
|--------------|-----------|
| CD44_high    | C2, C6, C7 |
| CD44_low     | C3, C4, C5 |
| Unsorted     | C1, C8, C9 |

Only **CD44_high** and **CD44_low** samples were used for DE and ML.

---

# 🔬 Scientific Overview of the Analysis

### **Preprocessing**
- Processed 33,572 genes × 9 samples  
- Removed annotation fields (`geneType`, `numGeneId`)  
- Ensured all genes were unique and numeric  
- Constructed a metadata table with CD44 phenotypes  
- Verified alignment between counts and metadata

### **Differential Expression (DESeq2)**  
The contrast `CD44_high vs CD44_low` was performed using:

design = ~ condition


Key details:
- DESeq2 normalization via size factors  
- Filtering of low-count genes  
- Wald test + Benjamini–Hochberg adjustment  
- Exported full results as CSV  

### **Results**
- Hundreds of DEGs at FDR < 0.05  
- CD44 itself (and related pathways) appeared among top genes  
- Clear transcriptional separation between high and low groups

---

# 🔧 Workflow Summary (Technical)

## **1. Preprocessing — Notebook 01**
- Loads raw HTSeq count matrix  
- Removes non-count annotation columns  
- Builds structured `gene × sample` matrix  
- Creates metadata table  
- Outputs:
  - `GSE113660_counts.csv`
  - `GSE113660_metadata.csv`

---

## **2. Differential Expression (Notebook 02)**
Using **DESeq2**:

- Normalization  
- Modeling  
- Significance testing  
- Export of DE table  

Outputs:
- `GSE113660_deseq2_results.csv`
- `volcano_CD44_high_vs_low.png`

Significance criteria:
- `padj < 0.05`
- `|log2FC| > 1`

---

## **3. PCA & Heatmaps (Notebook 03)**
Generated:

- PCA (top 1000 variable genes)  
- Heatmap (top 50 DE genes)  
- Clustermap (z-score normalized)

Saved under `/figures`.

---

## **4. Machine Learning Classifier (Notebook 04)**
A **Random Forest** classifier trained on the top DE genes.

- Standardization  
- **LOOCV evaluation**  
- Extraction of top gene importances  

Outputs:
- `ml_predictions_loocv.csv`
- `feature_importance_randomforest.csv`
- ROC, confusion matrix, and importance plots

This demonstrated that CD44 phenotype is learnable from transcriptomic patterns.

---

# 📁 Repository Structure

```bash
├── data/
│   ├── raw/              # Raw GEO files
│   └── processed/        # Cleaned count matrix + metadata
│
├── results/              # DESeq2 results, ML predictions, importances
├── figures/              # Volcano, PCA, heatmaps, ROC, etc.
│
├── notebooks/
│   ├── 01_preprocessing.ipynb
│   ├── 02_deseq2_DE_analysis.ipynb
│   ├── 03_visualization_pca_heatmap.ipynb
│   └── 04_ml_classifier.ipynb
│
├── env/
│   ├── environment.yml   # Conda environment
│   └── requirements.txt  # pip dependencies
│
├── .gitignore
└── README.md
```

---

# ⚙️ Installation

### **Using Conda (recommended)**

```bash
conda env create -f env/environment.yml
conda activate cd44-rnaseq
```
### **Using pip**
```bash
pip install -r env/requirements.txt
```

---

# 🔁 Reproducing the Analysis

1. Clone this repository:

```bash
git clone https://github.com/Melikagr/GSE113660-cd44-rnaseq-analysis.git
cd GSE113660-cd44-rnaseq-analysis
```

2. Install the environment

3. Run the notebooks in order:

```bash
01 → 02 → 03 → 04
```


All results will be saved automatically to /results and /figures.

---

# 📊 Key Results
### Differential Expression

- Strong transcriptional contrast between CD44_high and CD44_low

- Volcano plot indicates many high-confidence DE genes

### Dimensionality Reduction

- PCA shows clear separation between the two phenotypes

### Machine Learning

- LOOCV evaluation

- High accuracy and AUC

- Feature importance highlights key phenotype-associated genes

---

# 🖼 Example Figures

```bash
figures/volcano_CD44_high_vs_low.png
figures/pca_CD44.png
figures/heatmap_top50_DE_genes.png
figures/roc_CD44_classifier.png
```

--- 

# 📎 References

- Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology (2014).

- GEO Accession GSE113660:

    🔗 https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE113660

---

# 💬 Final Note

Although this is an independent learning project, I built it with the same rigor and structure I would apply in a research setting.
It represents both my current skills and the direction I want to grow — bridging biological intuition with computational analysis.

I hope this repository demonstrates my capability and commitment to pursuing advanced study in bioinformatics, computational biology, and biomedical data science.
