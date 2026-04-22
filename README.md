# 🧬 Precision Oncology: Multi-Omic Integration for Drug Sensitivity Prediction

### 🎯 Project Impact
This framework addresses the challenge of **Translational Integration** in oncology. By bridging the gap between patient-derived multi-omics (TCGA) and cell-line therapeutic response (DepMap), this pipeline identifies robust, explainable biomarkers for drug sensitivity in breast cancer.

---

### 🚀 Key Findings
* **Biomarker Identification:** Identified **IGKV1-5 and ALPL** as critical predictors for drug sensitivity using SHAP (Explainable AI).
* **Biological Validation:** Pathway enrichment confirmed that high-performing features correlate with **Immunoglobulin complexes and Apoptosis** pathways.
* **Single-Cell Resolution:** Successfully mapped the tumor microenvironment into 9+ distinct cell types, validating clusters with canonical markers (*CD8A, HLA-DRA*).

---

### 📊 Results Preview
| Cell Type Mapping (scRNA-seq) | Feature Importance (SHAP) |
|---|---|
| ![Cell Type UMAP](Result/umap_celltype_clean.jpg) | ![SHAP Summary](Result/shap_summary_bar.png) |

---

### 🔍 Analytical Workflow

#### 1. Data Acquisition & Integration
* **scRNA-seq:** 10x Genomics data curated from GEO.
* **Genomics (CNV/Mutation):** Curated via GDC API from **TCGA**.
* **Pharmacology:** Large-scale screening data from **DepMap (GDSC)**.
* **Alignment:** Implemented a custom mapping strategy to align patient IDs across disparate multi-omic modalities.

#### 2. Explainable Machine Learning (XAI)
Instead of a "black-box" model, I utilized a **Random Forest Classifier** coupled with **SHAP values** to move from prediction to interpretation.
* **Global Interpretation:** Quantified feature importance across the entire cohort to identify primary drivers.
* **Local Interpretation:** Generated force plots to understand per-sample sensitivity drivers, laying the groundwork for personalized medicine.

#### 3. Systems Biology & Enrichment
* **Functional Profiling:** Integrated **gProfiler2** to translate ML-identified features into biological pathways.
* **Network Analysis:** Visualized gene-gene interactomes via **STRING** and **Cytoscape** to identify central hub genes within predictive modules.

---

### 🛠️ Infrastructure & Reproducibility
To ensure production-grade reliability, the entire workflow is containerized and orchestrated:
* **Nextflow:** Manages the polyglot workflow (Python/R) and ensures execution scalability and checkpointing.
* **Docker:** Encapsulated all dependencies (Scanpy, Scikit-learn, SHAP, gProfiler2) for seamless "one-command" execution.

#### To Run Locally:
```bash
# Clone the repository
git clone [https://github.com/Rishika-Madhanagopal/Oncology_pipeline.git](https://github.com/Rishika-Madhanagopal/Oncology_pipeline.git)
cd Oncology_pipeline

# Execute the pipeline
nextflow run main.nf -profile docker

---

## 📁 Project Structure

```bash
Oncology_pipeline/
├── data/                      # Raw and processed datasets (ignored in repo)
├── scripts/                   # Python and R scripts for each phase
├── models/                    # Saved ML models
├── Result/                    # Figures, SHAP plots, enrichment results
├── Network_analysis/          # Cytoscape files
├── Dockerfile                 # Reproducible environment container
├── environment.yml            # Conda environment specification
├── nextflow.config            # Nextflow configuration
├── main.nf                    # Main Nextflow pipeline
├── .gitignore                 # Large files ignored from Git
└── README.md
```


---

## 🔄 Workflow Overview

### ✅ Phase 1: Data Acquisition
- **scRNA-seq**: Downloaded from GEO (10x Genomics format)
- **Mutations/CNV**: Retrieved from TCGA via GDC API
- **Drug Sensitivity**: Extracted from DepMap (GDSC format)

### ✅ Phase 2: Preprocessing
- `scripts/organize_samples_scRNA.py`: Organize scRNA-seq folders
- `scripts/preprocess_scrna.py`: Quality control, normalization, PCA, clustering using Scanpy
- `scripts/GDC_mutation.py`, `GDC_cnv.py`, `demap.py`: Format mutation, CNV, and drug datasets

### ✅ Phase 3: Integration & Feature Engineering
- Align samples across datasets
- Normalize features, scale expression
- Output: `data/master_features_scaled.csv`

### ✅ Phase 4: Machine Learning
- Train **RandomForestClassifier** on selected drug targets
- Evaluate using cross-validation (AUC, confusion matrix)
- Save model: `models/model_rf_<drug>.pkl`

### ✅ Phase 5: Model Interpretation
- Use **SHAP** to generate global and per-sample explanations
- Save:
  - SHAP summary plot (`Result/shap_summary_bar.png`)
  - Force plot for local explanation
  - Top features: `data/top_predictive_features.csv`

### ✅ Phase 6: Network Analysis
- Functional enrichment with **gProfiler2**
- Visualize pathways and networks in Cytoscape
- Save: enrichment plot + Cytoscape session

---

## ⚙️ Reproducibility

This project is **fully reproducible** using:

- 📦 **Docker**: Environment encapsulated with all dependencies  
- 🧬 **Nextflow**: Pipeline management system for reproducible workflows

### To Run Locally:
```bash
# Install Nextflow and Docker
nextflow run main.nf -profile docker
