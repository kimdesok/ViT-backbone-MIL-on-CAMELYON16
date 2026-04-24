# 🧬 Training ViT-MILs on CAMELYON16 Datasets
## Overview

>* As a preliminary experiment, Vision Transformer(ViT)–based Multiple Instance Learning (MIL) model was trained on the CAMELYON16 dataset, and its performance benchmarked against the ResNet50-MIL model.  
>* The ViT-MIL approach seemed to demonstrate an improved robustness to weak WSI-level labeling and higher classification performance(no stat. test given).  As we saw a similar trend with the TCGA-BRCA dataset, this suggested that ViT-MIL models would outperform CNN-based MIL models on CAMELYON16 datasets, too.
>* CAMELYON16 datasets were prepared as TFRecord format for preprocessing and exploratory data analysis (EDA).
>* Further experiments with Camelyon17 are planned in the next project phase.

## Aims

1. Acquire and preprocess histopathology Whole-Slide Images (WSIs).
2. Perform Exploratory Data Analysis (EDA) and patch-level segmentation.
3. Train and compare MIL models with ViT vs. CNN backbone.
4. Design inference services for cancer diagnosis assistance.
5. Develop and operate an NPU-based prototype for real-time inference.

**Core tasks:** dataset construction, model training, inference design, Streamlit demo development, and REST-API–based prototype implementation.

## Dataset Preparation and Preprocessing

* Datasets currently prepared include TCGA-BRCA, PatchCamelyon, and Camelyon16 (Camelyon17 forthcoming).
* All training data were converted to TFRecord format to optimize I/O efficiency and support scalable multi-GPU training.
* Due to storage constraints (≈ 2 TB), simultaneous handling of all datasets was not possible; preprocessing each dataset requires several days.
* Future storage expansion to ≥ 5 TB is essential to support larger WSI datasets and long-term caching.

## Model Training and Evaluation (CAMELYON16, completed)
### Performance on CAMELYON16

All MIL models were trained using weakly labeled WSI-level data.
ViT-MIL outperformed ResNet50-MIL model, achieving AUC ≈ 0.989 compared to 0.937 for a ResNet50 variant (Table 1).

📊 Table 1. Comparison of ResNet- and ViT-backbone MIL models on CAMELYON16.
![alt text](/images/MIL-performance-table.png)


