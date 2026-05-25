# PANTHER-SSL-Fusion

Deep learning–based pancreatic tumor segmentation in diagnostic MRI using 3D nnU-Net, semi-supervised pseudo-label learning, and component-guided fusion.

---

# Deep Learning-Based Pancreatic Tumor Segmentation in Diagnostic MRI with Semi-Supervised Enhancements

---

## Overview

Pancreatic tumor segmentation in diagnostic MRI is a challenging medical image analysis task due to low tumor contrast, irregular tumor boundaries, anatomical variability, and limited availability of expert-annotated data. This project explores a deep learning–based segmentation framework using the PANTHER Challenge dataset.

The project first trains supervised 3D nnU-Net models on labeled T1-weighted MRI data. Then, unlabeled MRI scans are used through pseudo-label generation and filtering. The filtered pseudo-labels are combined with the original labeled dataset for semi-supervised retraining. Finally, a component-guided fusion strategy is applied to combine supervised and semi-supervised predictions, aiming to recover failure cases and improve inference reliability.

---

# Dataset

Dataset Source: PANTHER Challenge  
https://panther.grand-challenge.org/

The project uses **Task 1 diagnostic T1-weighted MRI data** containing:

- Labeled MRI scans
- Unlabeled MRI scans
- Ground truth pancreas and tumor annotations

Final evaluation was conducted on **92 labeled T1 MRI cases**.

---

# Methodology

The proposed pipeline consists of the following stages:

1. MRI preprocessing using the nnU-Net pipeline
2. Supervised 3D nnU-Net training
3. Pseudo-label generation on unlabeled MRI scans
4. Pseudo-label filtering
5. Semi-supervised retraining
6. Component-guided fusion
7. Quantitative and qualitative evaluation

---

# Pseudo-Label Filtering

To improve pseudo-label quality before retraining, a filtering strategy was applied:

- Majority voting across fold predictions
- Largest connected component retention for pancreas segmentation
- Pancreas-guided tumor gating using a dilated pancreas mask
- Removal of anatomically implausible tumor regions
- Removal of very small tumor components

This filtering process reduces noisy pseudo-labels and improves semi-supervised retraining stability.

---

# Component-Guided Fusion

The final prediction is generated through a component-guided fusion strategy that combines supervised and semi-supervised outputs.

The fusion process:

- Preserves stable tumor regions from supervised predictions
- Recovers missed tumors from semi-supervised predictions
- Removes anatomically implausible components
- Reduces false positives
- Improves segmentation reliability

Unlike simple probability averaging, the proposed fusion operates at the connected-component level.

---

# Models Evaluated

| Model | Description |
|---|---|
| old_fold0 | Supervised 3D nnU-Net fold 0 |
| old_fold1 | Supervised 3D nnU-Net fold 1 |
| old_fold2 | Supervised 3D nnU-Net fold 2 |
| new_fold0 | Semi-supervised retrained fold 0 |
| new_fold1 | Semi-supervised retrained fold 1 |
| new_fold2 | Semi-supervised retrained fold 2 |
| unionF2 | Union fusion |
| guidedF2 | Component-guided fusion |

---

# Evaluation Metrics

The following metrics were used:

- Dice Similarity Coefficient
- Intersection over Union
- Precision
- Recall
- F1-score
- Hausdorff Distance 95
- Average Surface Distance
- Normalized Surface Dice
- Boundary Error
- Relative Volume Difference
- Relative Absolute Volume Difference

---

# Main Results

Evaluation was performed on 92 labeled T1 MRI cases.

| Model | Class | Dice | IoU | Precision | Recall | F1 |
|---|---|---:|---:|---:|---:|---:|
| old_fold2 | Tumor | 0.8508 | 0.7520 | 0.8479 | 0.8655 | 0.8566 |
| new_fold2 | Tumor | 0.8076 | 0.6829 | 0.8237 | 0.8143 | 0.8189 |
| unionF2 | Tumor | 0.8423 | 0.7441 | 0.8406 | 0.8619 | 0.8511 |
| guidedF2 | Tumor | 0.8561 | 0.7534 | 0.8451 | 0.8794 | 0.8619 |

The component-guided fusion model achieved the most reliable tumor segmentation performance.

---

# Key Findings

- Supervised models achieved strong average Dice scores but occasionally failed completely on difficult tumor cases.
- Semi-supervised retraining reduced catastrophic tumor omission cases.
- Pseudo-label filtering improved pseudo-mask quality.
- Component-guided fusion recovered missed tumors and reduced noisy detections.
- Reliability improved even when average Dice improvements were modest.

---

# Qualitative Results

The framework demonstrated:

- Recovery of tumors missed by supervised models
- Reduction of noisy tumor boundaries
- Improved overlap with ground truth
- Better robustness across difficult cases

Representative qualitative examples include:

- Complete tumor recovery after semi-supervised retraining
- Fusion-based cleanup of fragmented tumor predictions
- Improved boundary consistency across axial, sagittal, and coronal views

---



# Main Dependencies

```text
python
pytorch
nnunetv2
numpy
pandas
scipy
simpleitk
nibabel
matplotlib
scikit-image
tqdm
```

---

# Training Pipeline

## Step 1: Prepare Dataset

Convert the PANTHER Task 1 MRI dataset into nnU-Net format.

Expected structure:

```text
Dataset110_PANTHER_T1/
├── imagesTr/
├── labelsTr/
├── imagesTs/
└── dataset.json
```

---

## Step 2: Train Supervised Models

```bash
nnUNetv2_plan_and_preprocess -d 110 --verify_dataset_integrity

nnUNetv2_train 110 3d_fullres 0
nnUNetv2_train 110 3d_fullres 1
nnUNetv2_train 110 3d_fullres 2
```

---

## Step 3: Generate Pseudo Labels

Run pseudo-label generation:

```text
notebooks/pseudo_label_generation.ipynb
```

---

## Step 4: Filter Pseudo Labels

Run pseudo-label filtering:

```text
notebooks/pseudo_label_filtering.ipynb
```

---

## Step 5: Semi-Supervised Retraining

```text
notebooks/semi_supervised_training.ipynb
```

---

## Step 6: Component-Guided Fusion

```text
notebooks/component_guided_fusion.ipynb
```

---

## Step 7: Evaluation

```text
notebooks/evaluation_analysis.ipynb
```

---

# Result Files

Example evaluation files:

```text
summary_by_model_class_cleaned.csv
per_instance_metrics_cleaned.csv
cv_per_case_dice_oldF.csv
```

---

# Contributions

This project focuses not only on improving average segmentation accuracy but also on improving segmentation reliability and reducing catastrophic failure cases.

Main contributions include:

- Semi-supervised pancreatic tumor segmentation using filtered pseudo-labels
- Anatomy-aware pseudo-mask filtering
- Component-guided fusion for reliable inference
- Failure-case recovery analysis
- Reliability-focused evaluation

---

# Limitations

- Only T1-weighted MRI data were used
- No external clinical validation dataset
- Pseudo-label quality affects retraining
- Fusion increases computational complexity
- Intended for research use only

---

# Future Work

Potential future improvements include:

- Multi-modal MRI integration
- Uncertainty-aware semi-supervised learning
- Transformer-based fusion methods
- External dataset validation
- Faster inference optimization
- Clinical deployment studies

---

# References

[1] P. Rawla, T. Sunkara, and V. Gaduputi, “Epidemiology of pancreatic cancer: Global trends, etiology and risk factors,” World Journal of Oncology, vol. 10, no. 1, pp. 10–27, 2019.

[2] O. Ronneberger, P. Fischer, and T. Brox, “U-Net: Convolutional Networks for Biomedical Image Segmentation,” MICCAI, 2015.

[3] F. Isensee et al., “nnU-Net: A self-configuring method for deep learning-based biomedical image segmentation,” Nature Methods, 2021.

[4] W. Liang et al., “Deep learning for segmentation of pancreatic tumors on multi-parametric MRI,” International Journal of Radiation Oncology, Biology, Physics, 2020.

[5] PANTHER Challenge. Available: https://panther.grand-challenge.org/

[6] DIAGNijmegen PANTHER Baselines. Available: https://github.com/DIAGNijmegen/PANTHER_baseline

---

# Citation

If you use this repository or build upon this project, please cite the PANTHER Challenge dataset and the nnU-Net framework.

---

# Disclaimer

This repository is intended for academic and research purposes only. It is not a clinically approved medical system.
