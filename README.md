# Full-Size CT Pulmonary Nodule Analysis Using 2.5D Swin-UNet and Adaptive Attention

This repository contains the implementation and experimental analyses of a deep learning framework for pulmonary nodule segmentation and slice-level nodule-presence classification using CT images from the LIDC-IDRI dataset.

The framework combines a 2.5D Swin-UNet with adaptive attention mechanisms for pulmonary nodule segmentation and an XGBoost-based classifier for identifying CT slices containing pulmonary nodules. The repository also includes preprocessing procedures, ablation studies, quantitative evaluation, statistical analysis, model calibration, and explainable AI (XAI) analyses.

## Project Overview

The framework consists of the following main stages:

1. **Dataset Preprocessing:** Preparation of positive and negative datasets from the LIDC-IDRI dataset.
2. **Pulmonary Nodule Segmentation:** A 2.5D Swin-UNet architecture with adaptive attention mechanisms is used to segment pulmonary nodules in CT slices.
3. **Nodule-Presence Classification:** Deep features extracted from the segmentation model are combined with handcrafted features and used as input to an XGBoost classifier.
4. **Model Evaluation and Analysis:** Quantitative evaluation, ablation experiments, statistical analyses, and qualitative examination of model predictions.
5. **Explainable AI:** Attention analysis, Grad-CAM, occlusion sensitivity, and SHAP are used to investigate model behavior and feature contributions.

## Dataset

The experiments use the **Lung Image Database Consortium and Image Database Resource Initiative (LIDC-IDRI)** dataset.

The dataset contains thoracic CT scans collected from multiple institutions and annotated by experienced radiologists.

Dataset reference:

- [LIDC-IDRI – The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/collection/lidc-idri/)

The CT images and annotations were processed to generate resized images and corresponding masks. Patient-level data splitting was used in the experiments to prevent patient overlap between training, validation, and test sets.

## Methodology

### 1. Dataset Preprocessing

The preprocessing stage prepares the positive and negative datasets used in the segmentation and classification tasks.

- The positive segmentation dataset is used exclusively for pulmonary nodule segmentation.
- The positive and negative datasets are used together for slice-level nodule-presence classification.
- CT images are converted to Hounsfield Units (HU), clipped to the range of −1000 to 400 HU, normalized, and resized to 320 × 320 pixels.
- Corresponding segmentation masks are generated during preprocessing.

The preprocessing code is provided in the root-level `Preprocessing/` directory.

### 2. Pulmonary Nodule Segmentation

The segmentation stage uses a 2.5D Swin-UNet architecture with adaptive attention mechanisms.

Main characteristics:

- Swin Transformer-based encoder.
- 2.5D input representation using three consecutive axial slices: previous, central, and next.
- Full-size input images of 320 × 320 pixels.
- Adaptive attention modules incorporating spatial, local, and global information.
- Decoder attention.
- Deep supervision during training.

The central axial slice is used as the segmentation target, while neighboring slices provide additional contextual information.

### 3. Segmentation Ablation Studies

The segmentation experiments investigate the contribution of different input representations, attention configurations, and loss functions.

The main experiments include:

- **Main Ablation Study:** Comparison of five model configurations, including 2D and 2.5D baselines and different attention arrangements.
- **Loss Function Ablation Study:** Evaluation of different loss-function configurations.
- **Patch-Based Ablation Study:** Investigation of different input patch sizes.

The Main Ablation Study evaluates five model configurations using five random seeds (41, 42, 43, 44, and 45).

### 4. Slice-Level Nodule-Presence Classification

The classification stage identifies whether a CT slice contains a pulmonary nodule.

An XGBoost classifier is trained using a combination of deep features and handcrafted features.

The feature representation contains 776 features:

- **768 deep features:** Global average pooled encoder features extracted from the segmentation model.
- **8 handcrafted features:** Image intensity statistics, predicted-mask characteristics, and intensity statistics within the predicted-mask region.

The classification task is performed at the slice level and focuses on nodule presence or absence.

### 5. Model Evaluation and Statistical Analysis

The segmentation and classification stages are evaluated using quantitative metrics and statistical analyses.

The analyses include:

- Segmentation evaluation using Dice score and HD95.
- Classification evaluation using ROC-AUC, sensitivity, specificity, and other classification metrics.
- Patient-level statistical analysis and confidence intervals.
- Comparison of different model configurations.
- Qualitative examination of segmentation results and prediction errors.
- Model calibration analysis.

### 6. Explainable AI Analysis

Explainability analyses are performed for both segmentation and classification.

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
├── Preprocessing/
│   └── Preprocessing.ipynb
│
├── Segmentation/
│   ├── Main_Ablation_Study/
│   ├── Loss_Function_Ablation_Study/
│   ├── Patch_Based_Ablation_Study/
│   ├── Results/
│   │   ├── CSV/
│   │   └── Figures/
│   ├── Segmentation_Results_Analysis.ipynb
│   └── README.md
│
├── Classification/
│   ├── Results/
│   │   ├── CSV/
│   │   └── Figures/
│   └── Classification_Results.ipynb
│
├── XAI_Analysis/
│   ├── Results/
│   │   ├── CSV/
│   │   └── Figures/
│   └── XAI_Analysis_and_Results.ipynb
│
├── requirements.txt
└── README.md
```

The directory tree provides an overview of the main project components. Additional experiment-specific files and result directories may be present within the corresponding folders.

## Directory Description

| Directory / Notebook | Description |
|---|---|
| `Preprocessing/` | Positive and negative dataset preparation. |
| `Preprocessing.ipynb` | Preprocessing code for generating the positive and negative datasets. |
| `Segmentation/` | Segmentation implementation, experiments, and result analysis. |
| `Main_Ablation_Study/` | Main segmentation experiments involving five model configurations and five random seeds. |
| `Loss_Function_Ablation_Study/` | Experiments investigating different loss-function configurations. |
| `Patch_Based_Ablation_Study/` | Experiments evaluating patch-based segmentation configurations. |
| `Segmentation_Results_Analysis.ipynb` | Quantitative evaluation, statistical analysis, and visualization of segmentation results. |
| `Classification/` | Slice-level nodule-presence classification implementation and evaluation. |
| `Classification_Results.ipynb` | Classification evaluation and related analyses. |
| `XAI_Analysis/` | Explainability analyses for segmentation and classification. |
| `XAI_Analysis_and_Results.ipynb` | Attention analysis, Grad-CAM, occlusion sensitivity, and SHAP analyses. |
| `Results/` | Organization of generated numerical results and figures within the corresponding project sections. |

## Execution Order

Follow the execution order below when reproducing the experiments.

### Step 1. Dataset Preprocessing

Run the two preprocessing cells in the root-level `Preprocessing/Preprocessing.ipynb` notebook to generate the positive and negative datasets.

The LIDC-IDRI dataset must be available locally, and the dataset paths must be configured before execution.

### Step 2. Main Ablation Study Setup

**Important:** Before running the subsequent experimental and analysis sections across the project, execute Cells 1–7 of the Main Ablation Study notebook in order.

These cells define the main configuration, dataset preparation and patient-level splits, data loaders, loss functions, model architecture, and evaluation functions.

The variables, functions, and dataset partitions established in these cells are required by subsequent experimental and analysis sections.

### Step 3. Main Segmentation Experiments

Run the Main Ablation Study using the five random seeds:

- Seed 41
- Seed 42
- Seed 43
- Seed 44
- Seed 45

Each run evaluates all five model configurations. The data split remains fixed across runs.

### Step 4. Result Analysis

Run `Segmentation_Results_Analysis.ipynb` to perform the segmentation result evaluation, quantitative analysis, statistical comparisons, and visualization.

The classification and XAI notebooks can then be executed using their corresponding data, checkpoints, and required outputs.

**Note:** Some analysis cells depend on variables, functions, or data partitions defined in the first six cells of the Main Ablation Study notebook. Run those cells before executing the dependent sections.

### Results Organization

Experimental and analysis outputs are organized into the corresponding `Results` directories:

- **CSV files:** Numerical results, evaluation metrics, and analysis outputs.
- **Figures:** Generated plots, charts, and visualizations.
- **Segmentation:** Segmentation evaluation and analysis outputs.
- **Classification:** Classification metrics and evaluation results.
- **XAI Analysis:** Attention analysis, SHAP results, and occlusion visualizations.

The notebooks save their outputs in these directories to keep experimental results separate from implementation code.

## Files Not Included in the Repository

Due to their large storage requirements, the following files are not included in this GitHub repository:

- The original LIDC-IDRI dataset.
- Preprocessed CT image datasets and corresponding segmentation masks.
- Positive and negative datasets prepared for classification.
- Trained model checkpoints and saved weights.
- Extracted feature files.
- Excel files containing intermediate experimental results and training logs.
- Other large intermediate files generated during preprocessing and model training.

These files must be generated using the provided preprocessing and experimental code or obtained separately before running the corresponding experiments.

The LIDC-IDRI dataset is publicly available through [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/collection/lidc-idri/).

## Experimental Environment

The framework was implemented using Python and PyTorch.

The main experimental environment included:

- Python
- PyTorch 2.4.1
- CUDA 12.1
- cuDNN 9.1
- NVIDIA GeForce RTX 3070 Ti (8 GB VRAM)

The experiments used patient-level data splitting to reduce the risk of data leakage.

## Installation and Dependencies

Install the required Python packages using:

```bash
pip install -r requirements.txt
```

PyTorch and CUDA should be installed with versions compatible with the available GPU and CUDA environment. Refer to the [official PyTorch installation guide](https://pytorch.org/get-started/locally/) for the appropriate installation command.

Before running the notebooks, configure the dataset, output, and checkpoint paths according to the local environment.

## Reproducibility

To reproduce the experiments:

1. Obtain the LIDC-IDRI dataset from TCIA.
2. Run the preprocessing notebook to generate the required positive and negative datasets.
3. Configure the dataset and checkpoint paths.
4. Execute Cells 1–7 of the Main Ablation Study notebook.
5. Run the main segmentation experiments using seeds 41–45.
6. Run the corresponding result analysis, classification, and XAI notebooks.

Some experiments require trained checkpoints and extracted feature files. These files are not included in the repository and must be generated using the corresponding experimental procedures or provided separately.
