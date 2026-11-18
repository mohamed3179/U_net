# U-Net: Cell Segmentation with PyTorch

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red)
![Task](https://img.shields.io/badge/Task-Image%20Segmentation-green)
![Dataset](https://img.shields.io/badge/Dataset-PhC--C2DH--U373-orange)

![U-Net segmentation results](results/predictions.png)

A PyTorch implementation of the U-Net architecture for binary cell segmentation in phase-contrast microscopy images using the PhC-C2DH-U373 dataset from the Cell Tracking Challenge.

The objective is to automatically segment cell regions from microscopy images by distinguishing foreground (cells) from background.

## Table of Contents

- [Quick start](#quick-start)
- [Description](#description)
- [Network architecture](#network-architecture)
- [Dataset](#dataset)
- [Training](#training)
- [Evaluation](#evaluation)
- [Prediction](#prediction)
- [Results](#results)
- [Requirements](#requirements)
- [References](#references)

## Quick start

### Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/UNet-Cell-Segmentation.git
cd UNet-Cell-Segmentation
```

Install dependencies:

```bash
pip install torch torchvision numpy matplotlib scipy tifffile imagecodecs jupyter
```

### Run the notebook

```bash
jupyter notebook
```

Open:

```text
unet_phc_u373_(2).ipynb
```

Execute all cells sequentially to download the dataset, preprocess images, train the network, and evaluate segmentation performance.

## Description

This project implements U-Net for binary semantic segmentation of microscopy images.

The complete pipeline includes:

- Dataset downloading and loading.
- Image normalization.
- Binary mask generation from instance annotations.
- Boundary-aware pixel weighting.
- Data augmentation.
- U-Net model implementation.
- Model training using Adam.
- Validation using IoU and Dice.
- Segmentation visualization.
- Connected-component analysis.

## Network architecture

The network follows the original U-Net encoder-decoder structure.

| Layer | Channels |
|---|---:|
| Input | 1 |
| Encoder 1 | 64 |
| Encoder 2 | 128 |
| Encoder 3 | 256 |
| Encoder 4 | 512 |
| Bottleneck | 1024 |
| Decoder | 512, 256, 128, 64 |
| Output | 2 |

The model uses:
- Double convolution blocks.
- ReLU activation.
- Max pooling.
- Transposed convolution.
- Skip connections.
- Dropout.

## Dataset

The project uses the PhC-C2DH-U373 dataset.

Download: [Cell Tracking Challenge](http://data.celltrackingchallenge.net/training-datasets/PhC-C2DH-U373.zip)

The dataset contains phase-contrast microscopy images and corresponding segmentation annotations.

Dataset organization:

```text
PhC-C2DH-U373/
├── 01/
├── 02/
├── 01_GT/
│   └── SEG/
└── 02_GT/
    └── SEG/
```

The notebook uses 34 annotated frames:

- Training: 28 images.
- Validation: 6 images.

Images are normalized and converted into binary foreground/background masks.

## Training

The model is trained using weighted cross-entropy loss and the Adam optimizer.

Training configuration:

| Parameter | Value |
|---|---|
| Epochs | 40 |
| Batch size | 4 |
| Learning rate | 0.001 |
| Optimizer | Adam |
| Scheduler | CosineAnnealingLR |
| Input size | 256 × 256 |
| Classes | 2 |
| Random seed | 42 |

Data augmentation includes random crops, elastic deformation, rotations, flips, and intensity variations.

The best checkpoint is saved as:

```text
unet_phc_u373_best.pt
```

## Evaluation

The model is evaluated using two segmentation metrics.

**Intersection over Union (IoU)**

\[
IoU = \frac{TP}{TP+FP+FN}
\]

**Dice coefficient**

\[
Dice = \frac{2TP}{2TP+FP+FN}
\]

## Results

Best recorded validation performance:

| Metric | Score |
|---|---:|
| Validation IoU | 0.7602 |
| Validation Dice | 0.8626 |
| Training epochs | 40 |

### Visual results

Add your actual generated figures to the `results/` directory.

**Input microscopy image and ground truth**

![Dataset samples](results/dataset_samples.png)

**Training and validation curves**

![Training curves](results/training_curves.png)

**Predicted segmentation**

![Predictions](results/predictions.png)

**Connected components**

![Connected components](results/instance_segmentation.png)

## Prediction

The notebook includes an inference and visualization section to generate binary segmentation masks from microscopy images using the trained model.

The predicted foreground is obtained from the model's class probabilities.

A threshold of 0.5 is used for the visualization, followed by connected-component analysis.

## Requirements

- Python 3.10+
- PyTorch
- NumPy
- Matplotlib
- SciPy
- tifffile
- ImageCodecs
- Jupyter Notebook

## References

1. Ronneberger, O., Fischer, P., and Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. [Original Paper](https://arxiv.org/abs/1505.04597).

2. [Cell Tracking Challenge](http://celltrackingchallenge.net/).

3. [PyTorch Documentation](https://pytorch.org/docs/stable/index.html).

---

**Author:** [Your Name]

**Institution:** University Ibn Zohr

**Field:** Artificial Intelligence

**Project:** Deep Learning – Biomedical Image Segmentation
