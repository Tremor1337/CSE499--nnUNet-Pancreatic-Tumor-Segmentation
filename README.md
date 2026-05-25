# PANTHER-SSL-Fusion

Deep learning–based pancreatic tumor segmentation in diagnostic MRI using 3D nnU-Net, semi-supervised pseudo-label learning, and component-guided fusion.

---

# Deep Learning-Based Pancreatic Tumor Segmentation in Diagnostic MRI with Semi-Supervised Enhancements

---

## Overview

This repository contains the implementation and experimental analysis of a pancreatic tumor segmentation framework using diagnostic T1-weighted MRI scans from the PANTHER Challenge dataset. The project explores supervised and semi-supervised deep learning approaches using 3D nnU-Net and introduces a component-guided fusion strategy to improve segmentation reliability and recover difficult tumor cases.

The framework focuses on:

- Supervised 3D nnU-Net training
- Semi-supervised retraining using filtered pseudo-labels
- Cross-fold evaluation
- Component-guided fusion
- Reliability-focused inference analysis

---

# Dataset

Dataset Source: PANTHER Challenge  
https://panther.grand-challenge.org/

The project uses:

- Diagnostic T1-weighted MRI scans
- Labeled MRI data
- Unlabeled MRI data
- Pancreas and tumor segmentation masks

Final evaluation was conducted on **92 labeled T1 MRI cases**.

---

# Repository Structure

```text
PANTHER-SSL-Fusion/
│
├── 3-fold evaluation.ipynb
├── fin analysis.ipynb
├── multiclass-psuedomask.ipynb
├── train2.ipynb
├── weighted_nnUNet.ipynb
│
├── results/
├── figures/
├── README.md
└── LICENSE
```

---

# Notebook Descriptions

## train2.ipynb

Main supervised training notebook for 3D nnU-Net segmentation on labeled T1 MRI scans.

Main tasks:

- Data loading
- MRI preprocessing
- nnU-Net training
- Cross-validation setup
- Baseline segmentation experiments

---

## multiclass-psuedomask.ipynb

Pseudo-label generation and filtering pipeline for semi-supervised learning.

Main tasks:

- Pseudo-mask generation
- Connected-component filtering
- Pancreas-guided tumor filtering
- Removal of noisy tumor predictions
- Semi-supervised dataset preparation

---

## weighted_nnUNet.ipynb

Weighted and fusion-based nnU-Net experiments.

Main tasks:

- Weighted prediction fusion
- Component-guided fusion
- Reliability-focused tumor recovery
- Failure case correction
- Ensemble refinement

---

## 3-fold evaluation.ipynb

Cross-fold quantitative evaluation notebook.

Main tasks:

- Dice evaluation
- IoU computation
- Precision and recall analysis
- Hausdorff Distance evaluation
- Fold-wise performance comparison

---

## fin analysis.ipynb

Final qualitative and quantitative analysis notebook.

Main tasks:

- Per-case evaluation
- Failure-case visualization
- Axial, sagittal, and coronal comparisons
- Error map generation
- Final result analysis

---

# Methodology

The proposed pipeline consists of the following stages:

1. MRI preprocessing and normalization
2. Supervised 3D nnU-Net training
3. Pseudo-label generation on unlabeled MRI scans
4. Pseudo-label filtering
5. Semi-supervised retraining
6. Component-guided fusion
7. Cross-fold evaluation
8. Qualitative and quantitative analysis

---

# Pseudo-Label Filtering

To improve pseudo-label quality, a filtering process was applied before semi-supervised retraining.

The filtering pipeline includes:

- Majority voting across folds
- Largest connected component selection
- Pancreas-guided tumor gating
- Removal of anatomically implausible regions
- Small tumor component suppression

This process reduces noisy pseudo-labels and stabilizes semi-supervised training.

---

# Component-Guided Fusion

The final segmentation is generated using a component-guided fusion algorithm.

Instead of simple averaging, the fusion strategy:

- Preserves reliable supervised tumor regions
- Recovers missed tumors from semi-supervised predictions
- Removes noisy false positives
- Applies connected-component analysis
- Produces more stable and reliable inference

The fusion framework significantly reduces catastrophic tumor omission cases.

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

- Supervised models achieved strong average Dice scores but occasionally failed on difficult tumor cases.
- Semi-supervised retraining reduced catastrophic tumor omission cases.
- Pseudo-label filtering improved pseudo-mask quality.
- Component-guided fusion recovered missed tumors while suppressing noisy predictions.
- Reliability improved even when average Dice improvements were modest.

---

# Qualitative Analysis

The framework demonstrated:

- Recovery of tumors missed by supervised models
- Reduction of noisy tumor boundaries
- Better overlap with ground truth
- Improved robustness across difficult cases

Representative examples include:

- Supervised model completely missing the tumor while semi-supervised learning recovered it
- Fusion correcting fragmented tumor predictions
- Improved anatomical consistency across axial, sagittal, and coronal planes

---

# Releases

The repository releases section contains:

- Real-time inference application
- Web-based summary and visualization application
- Experimental model outputs
- Demo files and utilities

These applications are provided separately as compressed release packages.

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

# Installation

Clone the repository:

```bash
git clone https://github.com/your-username/PANTHER-SSL-Fusion.git
cd PANTHER-SSL-Fusion
```

Create virtual environment:

```bash
python -m venv venv
```

Activate environment:

```bash
# Windows
venv\Scripts\activate

# Linux/macOS
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# Running the Notebooks

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Run notebooks in the following order:

1. train2.ipynb
2. multiclass-psuedomask.ipynb
3. weighted_nnUNet.ipynb
4. 3-fold evaluation.ipynb
5. fin analysis.ipynb

---

# Contributions

Main contributions of this project include:

- Semi-supervised pancreatic tumor segmentation
- Pseudo-label filtering for reliable retraining
- Component-guided fusion strategy
- Reliability-focused evaluation
- Failure-case recovery analysis

---

# Limitations

- Only diagnostic T1 MRI data were used
- No external clinical validation dataset
- Semi-supervised performance depends on pseudo-label quality
- Fusion increases computational complexity
- Intended for research use only

---

# Future Work

Potential future improvements include:

- Multi-modal MRI integration
- Transformer-based segmentation models
- Uncertainty-aware pseudo-labeling
- Faster inference optimization
- External clinical validation
- Real-time clinical deployment

---

# References

[1] P. Rawla, T. Sunkara, and V. Gaduputi, “Epidemiology of pancreatic cancer: Global trends, etiology and risk factors,” *World Journal of Oncology*, vol. 10, no. 1, pp. 10–27, 2019.

[2] O. Ronneberger, P. Fischer, and T. Brox, “U-Net: Convolutional Networks for Biomedical Image Segmentation,” *MICCAI*, 2015.

[3] F. Isensee et al., “nnU-Net: A self-configuring method for deep learning-based biomedical image segmentation,” *Nature Methods*, 2021.

[4] W. Liang et al., “Deep learning for segmentation of pancreatic tumors on multi-parametric MRI,” *International Journal of Radiation Oncology, Biology, Physics*, 2020.

[5] PANTHER Challenge. Available: https://panther.grand-challenge.org/

[6] DIAGNijmegen PANTHER Baselines. Available: https://github.com/DIAGNijmegen/PANTHER_baseline

---

# Citation

If you use this repository or build upon this project, please cite the PANTHER Challenge dataset and nnU-Net framework.

---

# Disclaimer

This repository is intended for academic and research purposes only. It is not a clinically approved medical system.
