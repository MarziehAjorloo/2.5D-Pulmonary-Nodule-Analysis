# Pulmonary Nodule Segmentation

## Overview

This directory contains the implementation and experimental studies for pulmonary nodule segmentation using the LIDC-IDRI dataset.

The segmentation framework is based on a 2.5D Swin-UNet architecture with adaptive attention mechanisms and deep supervision. The experiments investigate the effects of input representation, attention configurations, and loss functions on segmentation performance.

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

For 2.5D configurations, the central axial slice is used as the segmentation target, while the neighboring slices provide additional contextual information.

## Dataset Preparation

The positive segmentation dataset is generated using the preprocessing code provided in the root-level `Preprocessing/` directory.

The dataset is loaded and filtered during the Main Ablation Study. The following criteria are applied:

- Samples with a malignancy score of 3 are excluded.
- Samples with a polygon area smaller than 100 pixels are excluded.

These filters are applied after the initial dataset preprocessing.

The 2.5D input stacks are also constructed during data loading in the Main Ablation Study. The preprocessing notebook generates individual 2D images and masks.

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

The notebook is executed separately for each seed. Each run evaluates all five model configurations.

The same patient-level data partitions are maintained across the different seeds and model configurations to support consistent comparisons.

### Patient-Level Data Splitting

The dataset is divided into training, validation, and test sets using patient-level splitting:

- Training: 70%
- Validation: 15%
- Test: 15%

Patient identifiers are used to prevent slices from the same patient from appearing in multiple subsets.

The data split is kept fixed across the experiments.

### Notebook Execution

The Main Ablation Study notebook contains seven cells:

- **Cells 1–6:** Configuration, dataset preparation, patient-level splitting, data loaders, loss functions, model architecture, and evaluation functions.
- **Cell 7:** Training and evaluation of the five segmentation model configurations.

Run Cells 1–7 sequentially. Repeat the execution of Cell 7 using seeds 41, 42, 43, 44, and 45. Each run evaluates all five model configurations.

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

The experiments examine how different loss configurations affect segmentation quality and boundary-related performance.

## Patch-Based Ablation Study

The Patch-Based Ablation Study investigates the effect of input patch size on pulmonary nodule segmentation.

Different patch configurations are evaluated to examine the effect of localized image inputs on segmentation performance.

This study is separate from the main full-size segmentation experiments.

## Segmentation Evaluation and Analysis

### Notebook: `Segmentation_Results_Analysis.ipynb`

This notebook contains the code for evaluating and analyzing the outputs of the segmentation experiments. It generates the corresponding numerical results, statistical analyses, tables, and figures.

The analysis includes:

- Quantitative evaluation of segmentation predictions.
- Dice score and other segmentation metrics.
- HD95 evaluation.
- Empty prediction analysis.
- Comparison of model configurations.
- Statistical analysis across experimental runs.
- Confidence interval calculations.
- Generation of tables and figures for reporting experimental results.

The generated CSV files and figures are stored in the corresponding `Results/CSV/` and `Results/Figures/` directories.

