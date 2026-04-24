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

## Exploratory Data Analysis
To extend the ViT-MIL framework validated on TCGA-BRCA [previously](https://github.com/kimdesok/ViT-backbone-MIL-on-TCGA/blob/main/README.md), we conducted a preliminary EDA on the **Camelyon16** dataset to prepare it for the training experiments.  
Unlike TCGA-BRCA, Camelyon16 provides **pixel-level tumor annotations**, enabling strongly supervised sampling and comparison against the weakly supervised ViT-MIL baseline(data not shown).  

<img width="536" height="280" alt="image" src="https://github.com/user-attachments/assets/ed9323a6-0ccd-4d3b-aeab-72b6d4bb5f80" /><br>
**Figure 1**. Tumor mask visualization of a Camelyon16 whole slide image(WSI), representing sentinel lymph node tissue obtained during breast cancer surgery.<br>  Unlike TCGA datasets, Camelyon16 provides pixel-level annotations, enabling patch- or region-level ground truth and allowing more granular positive-patch sampling for MIL.

<img width="321" height="412" alt="image" src="https://github.com/user-attachments/assets/56aad0e5-f049-463e-9659-c9814fbfba50" /><br>
**Figure 2**. Patches representing tissue, tumor, and background were visualized in green, orange, and red, respectively, from the same slide as Fig. 1.<br>
Slides were filtered to remove glass regions, retaining only tissue patches. Positive patches were sampled from tumor areas, while negatives were taken exclusively from negative slides. This approach reduced the original WSI data volume by roughly 10 : 1 after conversion to TFRecord.

<img width="546" height="470" alt="image" src="https://github.com/user-attachments/assets/f0dd4956-558f-4520-9522-0c14202d6757" /><br>
**Figure 3**. Representative WSIs highlighting the localization challenge of metastatic cancer detection. <br> 
Panel A shows a large metastatic area (1,077 positive patches); Panel B shows micrometastatic tissue (2 positive patches ≈ 0.1% of all tissue), illustrating the inherent difficulty of WSI-level classification and the need for MIL-based learning. <br>

<img width="423" height="253" alt="image" src="https://github.com/user-attachments/assets/17943aac-a451-459d-af56-bfbad25cd12f" /><br>
**Figure 4**. Patch-level visualization of metastatic vs. normal tissue patches (tumor areas highlighted in red).<br>
Even to non-experts, visual cues are distinctive, though the severe class imbalance (tumor ≪ normal) complicates conventional classification.
To address this, segmentation-based approaches (e.g., U-Net, Mask RCNN) have achieved AUC > 97% [1][2], but require dense pixel-level labels.

## Model Training and Evaluation (CAMELYON16, completed)
### Performance on CAMELYON16

All MIL models were trained using weakly labeled WSI-level data.
ViT-MIL outperformed ResNet50-MIL model, achieving AUC ≈ 0.989 compared to 0.937 for a ResNet50 variant (Table 1).

📊 Table 1. Comparison of ResNet- and ViT-backbone MIL models on CAMELYON16.
![alt text](/images/MIL-performance-table.png)

The upcoming experiment will include a direct comparison between **weakly** and **strongly supervised** learning frameworks for WSI classification(planned but no execution yet).

## Development of a Prototype of Inference Service Platform (Planned)
Given the massive file sizes (up to 10 GB per WSI), efficient inference on ATOM NPU hardware requires compact model architectures and memory-aware pipelines.
Both ViT-MIL and CNN-MIL models have manageable footprints, confirming feasibility of NPU deployment.
A prototype of inference service platform has been designed and will be implemented partly by integrating:

>* REST API endpoints for automated WSI dataset processing,
>* Batch compilation of GPU trained models to NPU models
>* Inference and performance reports that include accuracy and throughput indicators
>* Streamlit-based visualization for real-time interpretability, and
>* Grad-CAM / attention-based heatmaps for clinical insight during the next stage development.

## Key Achievements

1) Trained and benchmarked ViT-MIL and CNN-MIL models on CAMELYON16 WSIs.
2) Demonstrated 2–3% AUC improvement of ViT-MIL over CNN-based MIL.
3) Achieved stable training of ViT-MIL under weak labeling without OOM failures.
4) Established scalable TFRecord preprocessing pipeline for Camelyon16 datasets.
5) Prepared groundwork for NPU-based inference deployment and multimodal extension with NLP-based pathology reporting.

## References
[1] S. Wu et al, An artificial intelligence model for detecting pathological lymph node metastasis in prostate cancer using whole slide images: a retrospective, multicentre, diagnostic study, eClinicalMedicine, 71:102580, Apr 5 2024 <br>
[2] J. Li et al, Improving the speed and quality of cancer segmentation using lower resolution pathology images,  83:11999–12015, June 29 2023
