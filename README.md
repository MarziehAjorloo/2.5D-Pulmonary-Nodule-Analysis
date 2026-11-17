# Full-Size CT Pulmonary Nodule Detection Using 2.5D Swin-UNet and Adaptive Decoder Attention

This repository contains the implementation and experimental analyses of a deep learning framework for pulmonary nodule segmentation and slice-level nodule-presence classification in CT images from the LIDC-IDRI dataset.

The proposed framework combines a 2.5D Swin-UNet with adaptive attention mechanisms for pulmonary nodule segmentation and an XGBoost-based classifier for identifying slices containing pulmonary nodules. The project also includes quantitative evaluation, ablation studies, error analysis, model calibration, and explainable AI (XAI) analyses.

## Project Overview

Pulmonary nodule analysis in CT images involves challenges related to nodule size, shape, appearance, and surrounding lung tissue. This project investigates a two-stage framework that combines segmentation and classification to support automated pulmonary nodule analysis.

The framework consists of the following main stages:

1. **Pulmonary Nodule Segmentation:** A 2.5D Swin-UNet architecture with adaptive attention mechanisms is used to segment pulmonary nodules in CT slices.
2. **Nodule-Presence Classification:** Deep features extracted from the segmentation model are combined with handcrafted image and predicted-mask features and used as input to an XGBoost classifier. The classifier determines whether a CT slice contains a pulmonary nodule.
3. **Model Evaluation and Analysis:** The framework is evaluated using quantitative metrics, statistical analyses, ablation experiments, and qualitative visualizations.
4. **Explainable AI:** Attention analysis, Grad-CAM, occlusion sensitivity, and SHAP are used to investigate model behavior and feature contributions.

## Dataset

The experiments use the **Lung Image Database Consortium and Image Database Resource Initiative (LIDC-IDRI)** dataset.

The dataset contains thoracic CT scans collected from multiple institutions and annotated by experienced radiologists. It provides CT images and radiologist annotations that support the development and evaluation of pulmonary nodule analysis methods.

Dataset reference:

- [LIDC-IDRI – The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/collection/lidc-idri/)

The dataset was processed to generate resized CT slices and corresponding segmentation masks. Patient-level data splitting was used to prevent patient overlap between training, validation, and test sets.

## Methodology

### 1. Pulmonary Nodule Segmentation

The segmentation stage uses a 2.5D Swin-UNet architecture with adaptive attention mechanisms.

**Main characteristics:**

- Swin Transformer-based encoder.
- 2.5D input representation using three consecutive axial slices (previous, central, and next).
- Full-resolution input images of 320 × 320 pixels.
- Adaptive attention modules incorporating spatial, local, and global information.
- Decoder attention for improved feature representation and segmentation.
- Deep supervision during training.

The central axial slice is used as the target for segmentation, while its neighboring slices provide additional contextual information.

### 2. Segmentation Ablation Studies

Several experiments were conducted to investigate the contribution of different architectural and training components.

The main experiments include:

- **Main Ablation Study:** Comparison of 2D and 2.5D configurations and different attention arrangements.
- **Loss Function Ablation Study:** Evaluation of different loss-function configurations.
- **Patch-Based Ablation Study:** Investigation of the effect of different input patch sizes on segmentation performance.

### 3. Slice-Level Nodule-Presence Classification

The classification stage identifies whether a CT slice contains a pulmonary nodule.

An XGBoost classifier is trained using a combination of deep features and handcrafted features.

The feature representation contains 776 features:

- **768 deep features:** Global average pooled encoder features extracted from the segmentation model.
- **8 handcrafted features:** Image intensity statistics, predicted-mask characteristics, and image statistics within the predicted-mask region.

The classification task is performed at the slice level and focuses on nodule presence or absence.

### 4. Model Evaluation and Statistical Analysis

The segmentation and classification stages are evaluated using quantitative performance metrics and statistical analyses.

The analyses include:

- Segmentation performance evaluation using Dice score and HD95.
- Classification performance evaluation using ROC-AUC, sensitivity, specificity, and other classification metrics.
- Patient-level statistical analysis and confidence intervals.
- Comparison of different model configurations.
- Qualitative examination of segmentation results and prediction errors.
- Model calibration analysis.

### 5. Explainable AI Analysis

Explainability analyses are performed for both segmentation and classification stages.

**Segmentation explainability:**
- Decoder attention visualization.
- Grad-CAM analysis.
- Occlusion sensitivity analysis.
- Quantitative attention alignment with ground-truth nodule regions.

**Classification explainability:**
- SHAP feature importance analysis.
- SHAP-based feature group analysis.
- Occlusion sensitivity analysis.
- Evaluation of feature importance and stability across model runs.

## Repository Structure

```text
2.5D-Pulmonary-Nodule-Analysis/
│
├── Segmentation/
│   ├── Segmentation_Results.ipynb
│   │
│   ├── Main_Ablation_Study/
│   ├── Loss_Function_Ablation_Study/
│   └── Patch_Based_Ablation_Study/
│
├── Classification/
│   └── Classification_Results.ipynb
│
├── XAI_Analysis/
│   └── XAI_Analysis_and_Results.ipynb
│
└── README.md
```

### Directory Description

| Directory / Notebook | Description |
|---|---|
| `Segmentation/` | Main segmentation implementation and experimental results. |
| `Main_Ablation_Study/` | Experiments comparing the main segmentation architectures and attention configurations. |
| `Loss_Function_Ablation_Study/` | Experiments investigating different loss-function configurations. |
| `Patch_Based_Ablation_Study/` | Experiments evaluating patch-based segmentation configurations. |
| `Segmentation_Results.ipynb` | Main segmentation results, quantitative evaluation, and qualitative analysis. |
| `Classification/` | Classification implementation and experimental results. |
| `Classification_Results.ipynb` | Classification evaluation and related analyses. |
| `XAI_Analysis/` | Explainability analyses for the segmentation and classification stages. |
| `XAI_Analysis_and_Results.ipynb` | Attention analysis, Grad-CAM, occlusion sensitivity, and SHAP analyses. |

## Experimental Setup

The framework was implemented using Python and PyTorch.

The main experimental environment included:

- Python
- PyTorch 2.4.1
- CUDA 12.1
- cuDNN 9.1
- NVIDIA GeForce RTX 3070 Ti (8 GB VRAM)

The experiments used patient-level training, validation, and test splits to reduce the risk of data leakage.

## Reproducibility

The notebooks and experiment folders provide the code and analyses associated with the proposed framework.

To reproduce the experiments:

1. Obtain the LIDC-IDRI dataset from TCIA.
2. Prepare and preprocess the CT images and segmentation annotations according to the corresponding experimental setup.
3. Configure the dataset and checkpoint paths in the notebooks.
4. Run the relevant segmentation or classification experiments.
5. Use the results notebooks to reproduce the quantitative evaluations and visual analyses.

Some experiments require trained model checkpoints and preprocessed feature files. These files should be generated using the corresponding experimental procedures or provided separately when available.
