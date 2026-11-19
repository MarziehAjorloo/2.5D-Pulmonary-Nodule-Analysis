# Pulmonary Nodule Segmentation

## Overview

This directory contains the implementation and experimental analyses for pulmonary nodule segmentation using the LIDC-IDRI dataset.

The main objective is to investigate the effects of 2.5D input representations, adaptive attention mechanisms, and different loss-function configurations on segmentation performance.

The segmentation framework is based on a Swin-UNet architecture with deep supervision. The experiments compare different model configurations under consistent patient-level data partitions.

## Segmentation Architecture

The segmentation model uses a Swin Transformer-based encoder and a decoder with adaptive attention mechanisms.

Main characteristics:

- Swin Transformer-based encoder.
- Full-size input images of 320 × 320 pixels.
- 2D and 2.5D input configurations.
- Three consecutive axial slices for 2.5D input: previous, central, and next.
- Adaptive attention modules incorporating spatial, local, and global information.
- Decoder attention.
- Deep supervision through intermediate decoder outputs.

The central axial slice is used as the segmentation target, while neighboring slices provide additional contextual information in the 2.5D configuration.

## Dataset Preparation

The positive segmentation dataset is generated using the preprocessing code provided in the root-level `Preprocessing/` directory.

The segmentation experiments use the positive dataset containing annotated pulmonary nodules.

The dataset is loaded and filtered during the Main Ablation Study. The following criteria are applied during that stage:

- Samples with a malignancy score of 3 are excluded.
- A minimum polygon-area threshold of 100 pixels is applied.

These filters are applied after the initial dataset preprocessing.

## Main Ablation Study

The Main Ablation Study investigates the effects of input representation and attention configurations on segmentation performance.

### Model Configurations

Five configurations are evaluated:

| Configuration | Input | Encoder Attention | Decoder Attention |
|---|---|---|---|
| `2D_Baseline` | 2D | No | No |
| `2.5D_Baseline` | 2.5D | No | No |
| `2.5D_EncAtt` | 2.5D | Yes | No |
| `2.5D_DecAtt` | 2.5D | No | Yes |
| `2.5D_FullAtt` | 2.5D | Yes | Yes |

The 2D baseline uses a single axial slice as input. The 2.5D configurations use three consecutive axial slices.

### Random Seeds and Experimental Runs

Each of the five model configurations is evaluated using five random seeds:

- 41
- 42
- 43
- 44
- 45

The notebook is run separately for each seed. Each run evaluates all five model configurations.

The same patient-level data partitions are maintained across the different seeds and model configurations to support consistent comparisons.

### Patient-Level Data Splitting

The dataset is divided into training, validation, and test sets using patient-level splitting:

- Training: 70%
- Validation: 15%
- Test: 15%

Patient identifiers are used to prevent slices from the same patient from appearing in multiple subsets.

The data split is kept fixed across the experiments.

### Notebook

The Main Ablation Study notebook contains the configuration, dataset preparation, model implementation, training, and evaluation procedures.

Its main sections include:

1. Configuration and setup.
2. Dataset preparation and patient-level splitting.
3. Dataset loading, preprocessing, and DataLoaders.
4. Loss functions.
5. Model architecture.
6. Training and evaluation metrics.
7. Main ablation experiments.

**Important:** Execute Cells 1–7 of the Main Ablation Study notebook in order before running the subsequent experimental and analysis sections across the project.

These cells establish the shared configuration, dataset partitions, data loaders, loss functions, model architecture, and evaluation functions required by later experiments and analysis notebooks.

The main experiments use full-size images without cropping. Patch-based cropping is reserved for the Patch-Based Ablation Study.

## Loss Function Ablation Study

The Loss Function Ablation Study investigates the effects of different loss functions and loss combinations on segmentation performance.

The experiments compare alternative loss configurations under the segmentation framework.

The loss components used in the project include:

- Focal Tversky loss.
- Soft Dice loss.
- Weighted binary cross-entropy.
- Boundary loss.
- Auxiliary supervision losses for intermediate decoder outputs.
- Optional attention-map supervision.

The auxiliary decoder outputs use weighted binary cross-entropy losses for deep supervision.

The purpose of these experiments is to examine how different loss configurations affect segmentation quality and boundary-related performance.

## Patch-Based Ablation Study

The Patch-Based Ablation Study investigates the effect of input patch size on pulmonary nodule segmentation.

Different patch configurations are evaluated to examine the effect of localized image inputs on segmentation performance.

This study is separate from the main full-size segmentation experiments.

## Segmentation Results Analysis

### Notebook: `Segmentation_Results_Analysis.ipynb`

This notebook contains the result analysis and evaluation procedures for the segmentation experiments.

It is used to analyze the outputs generated by the Main Ablation Study and other segmentation experiments.

The analysis includes:

- Quantitative evaluation of segmentation predictions.
- Dice score and other segmentation metrics.
- HD95 evaluation.
- Empty prediction analysis.
- Comparison of model configurations.
- Statistical analysis across experimental runs.
- Confidence interval calculations.
- Generation of tables and figures for reporting experimental results.

## References

- [LIDC-IDRI – The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/collection/lidc-idri/)
- [PyTorch](https://pytorch.org/)
- [timm – PyTorch Image Models](https://github.com/huggingface/pytorch-image-models)
