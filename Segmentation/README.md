# Pulmonary Nodule Segmentation

## Overview

This directory contains the code for the pulmonary nodule segmentation experiments conducted using the LIDC-IDRI dataset. The main objective is to evaluate the effects of 2.5D input representations and adaptive attention mechanisms on segmentation performance.

The experiments use a Swin-UNet architecture with deep supervision and compare five model configurations under a consistent data split and evaluation protocol.

## Notebook

### `Ablation_study.ipynb`

This notebook implements the ablation study for the proposed segmentation framework. It covers dataset preparation, model configuration, training, validation, and evaluation.

The notebook is organized into the following sections.

### 1. Configuration and Setup

This section defines the main experimental settings, including:

- Random seed for the current experiment.
- Input image and mask directories.
- Image format and input patch size.
- Batch size, number of epochs, and number of data-loading workers.
- Number of input channels.
- Data usage percentage and computational device.

The input configuration supports both 2D and 2.5D experiments. In the 2.5D setting, the model receives the previous, central, and next CT slices as a three-channel input.

Each seed is run separately. The experiments use seeds 41–45, while the data split is kept fixed across runs.

### 2. Dataset Preparation and Patient-Level Splitting

This section loads the dataset metadata and selects samples for segmentation.

The segmentation dataset is filtered using the available nodule annotations. Samples with a malignancy score of 3 are excluded from this segmentation experiment, and a minimum polygon-area threshold of 100 pixels is applied.

The dataset is divided into training, validation, and test sets using patient-level splitting:

- Training: 70%
- Validation: 15%
- Test: 15%

Patient identifiers are used to prevent slices from the same patient from appearing in multiple subsets. The split uses a fixed data-splitting seed so that the same partitions can be used across the different model configurations and training seeds.

### 3. Dataset, Preprocessing, and DataLoaders

This section implements image and mask loading, 2.5D input construction, optional cropping, data augmentation, and DataLoader creation.

The main components include:

- Image and mask loading.
- Construction of three-slice inputs for 2.5D experiments.
- Handling of slice boundaries and patient transitions.
- Optional ROI-based or random cropping for patch-based experiments.
- Filtering of dataset indices.
- Horizontal and vertical flip augmentation during training.
- Weighted sampling to increase the sampling frequency of samples containing foreground pixels.
- Creation of training, validation, and test DataLoaders.

The main ablation experiments use full-size images without cropping. Cropping is reserved for experiments that explicitly investigate patch-based segmentation.

### 4. Loss Functions

This section defines the loss functions used to train the segmentation models.

The loss components include:

- Focal Tversky loss.
- Soft Dice loss.
- Boundary loss.
- Auxiliary supervision losses for intermediate decoder outputs.
- Optional attention-map supervision.

The combined loss is designed to account for foreground-background imbalance, region overlap, and boundary quality. Deep supervision provides additional training signals through intermediate outputs of the decoder.

### 5. Model Architecture

This section defines the Swin-UNet-based segmentation architecture and its supporting modules.

The main components include:

- A Swin Transformer encoder.
- Decoder upsampling blocks.
- Spatial attention.
- Adaptive attention with local and global branches.
- Encoder and decoder attention configurations.
- Deep supervision through intermediate decoder outputs.

The adaptive attention modules combine spatial, local, and global information. Their placement depends on the selected ablation configuration.

The model also retains intermediate activations, gradients, and attention information used by the explainability procedures developed in the project.

### 6. Training and Evaluation Metrics

This section defines the training and evaluation functions.

The evaluation metrics include:

- Dice score.
- Intersection over Union (IoU).
- Precision.
- Recall (sensitivity).
- Accuracy.
- Specificity.
- Area Under the ROC Curve (AUC).
- Average Precision.
- Hausdorff Distance at the 95th percentile (HD95).
- True positives (TP), true negatives (TN), false positives (FP), and false negatives (FN).

The evaluation functions also support threshold selection and evaluation at a specified threshold. HD95 is calculated when requested.

### 7. Main Ablation Study

This section trains and evaluates the five segmentation configurations:

| Configuration | 2.5D input | Encoder attention | Decoder attention |
|---|---|---|---|
| `2D_Baseline` | No | No | No |
| `2.5D_Baseline` | Yes | No | No |
| `2.5D_EncAtt` | Yes | Yes | No |
| `2.5D_DecAtt` | Yes | No | Yes |
| `2.5D_FullAtt` | Yes | Yes | Yes |

All configurations use the same patient-level data partitions. Each experiment is trained with the configured seed, and the best checkpoint is selected according to validation Dice at a threshold of 0.5.

The training procedure includes AdamW optimization, learning-rate scheduling, automatic mixed precision, and early stopping based on validation performance.

The notebook saves model checkpoints with the experiment name and seed in the filename. It also exports evaluation results to CSV and training logs to Excel.

## Experimental Design

The ablation study examines:

1. The contribution of adjacent CT slices by comparing 2D and 2.5D baselines.
2. The contribution of encoder attention.
3. The contribution of decoder attention.
4. The combined use of encoder and decoder attention.

The five configurations are evaluated under a consistent patient-level split to support comparison across architectures.

## Reproducibility

Before running the notebook, update the dataset paths in the configuration section to match the local environment.

The notebook is designed to run one seed at a time. To reproduce the five-seed experiments, use seeds 41, 42, 43, 44, and 45 while keeping the data split fixed.

The dataset and trained checkpoints are not included in this repository. They must be available locally before the corresponding experiments can be run.

## Current Status

This notebook documents the configuration, dataset preparation, model definition, loss functions, evaluation functions, and main ablation training loop currently included in the repository. Additional analysis and explainability sections will be documented as they are added.
