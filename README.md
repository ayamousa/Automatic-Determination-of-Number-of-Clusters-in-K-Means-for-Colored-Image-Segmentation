# 🖼️ Automatic Determination of Number of Clusters in K-Means for Colored Image Segmentation using PCA Eigen Vectors and Weighted Color Channel Variance

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green.svg)](https://opencv.org/)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-orange.svg)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Overview

This project presents a novel method for **automatically determining the optimal number of clusters `k`** in K-Means clustering for **colored image segmentation**. The approach leverages **PCA Eigen Vectors** and **Weighted Colour Channel Variance** in the **three colour spaces LAB, HSV,RGB** to eliminate the need for manual cluster selection.

Segmentation quality is evaluated using standard metrics: **Precision**, **Recall**, and **F-Measure**, computed against human-annotated ground truth masks.

The method was evaluated on **40 test images**, each with **5 ground truth annotations**, following standard benchmarking protocols used in the image segmentation literature.

---

## 📁 Repository Structure

```
├── segmentation_notebook.ipynb       # Main Google Colab notebook
├── segmentation_metrics.xlsx         # Quantitative results per image
├── SegmentedImages/                  # Output segmented images
│   ├── segmented_img_1_k3.jpg
│   ├── segmented_img_2_k4.jpg
│   └── ...
└── README.md                         # This file
```

---

## ⚙️ Methodology

### 1. Color Space Conversion
Images are converted from **RGB → CIE Lab** color space to improve perceptual uniformity and clustering performance.

### 2. Automatic k Determination (Core Contribution)
The function `FindOptimalk_xxB(img_idx)` automatically estimates the optimal number of clusters `k` for each image using:
- **PCA Eigen Vectors** — to capture dominant directions of color variance in the image
- **Weighted Color Channel Variance** — to weight the contribution of each Lab channel (L*, a*, b*) based on its discriminative power

This eliminates the need for manual `k` selection, making the method fully automatic.

### 3. K-Means Segmentation
```
Criteria : EPS + MAX_ITER (100 iterations, ε = 0.2)
Attempts : 10 random initializations
Init     : KMEANS_RANDOM_CENTERS
```

### 4. Evaluation
Each segmented image is compared against **5 ground truth masks** per image. Metrics are averaged across all ground truth annotations:

| Metric | Formula |
|--------|---------|
| Precision | TP / (TP + FP) |
| Recall | TP / (TP + FN) |
| F-Measure | 2 × (P × R) / (P + R) |

---

## 📊 Results

Quantitative results for all 43 images are stored in [`segmentation_metrics.xlsx`](segmentation_metrics.xlsx), including:

- Image index
- Optimal number of clusters `k`
- Per-image Precision, Recall, and F-Measure
- Overall averages (computed via Excel `AVERAGE` formulas)



## 🗂️ Dataset

This work uses the publicly available **Berkeley Segmentation Data Set 500 (BSDS500)**.

| Property | Details |
|----------|---------|
| **Dataset** | BSDS500 |
| **Test Images Used** | 40 images |
| **Ground Truth Masks** | 5 human annotations per image (215 masks total) |
| **Image Size** | 321 × 481 pixels |
| **Color Space Used** | CIE Lab |
| **Source** | UC Berkeley Computer Vision Group |
| **Access** | [https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html) |

### Dataset Citation
```bibtex
@inproceedings{arbelaez2011contour,
  title     = {Contour Detection and Hierarchical Image Segmentation},
  author    = {Arbelaez, Pablo and Maire, Michael and Fowlkes, Charless and Malik, Jitendra},
  journal   = {IEEE Transactions on Pattern Analysis and Machine Intelligence},
  volume    = {33},
  number    = {5},
  pages     = {898--916},
  year      = {2011},
  publisher = {IEEE}
}
```

---

## 🚀 How to Run

1. Open the notebook in Google Colab:
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

2. Mount Google Drive:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```

3. Run all cells in order. Results will be saved to:
   - 📊 `/content/drive/MyDrive/segmentation_metrics.xlsx`
   - 🖼️ `/content/drive/MyDrive/SegmentedImages/`

---

## 📦 Dependencies

```
opencv-python
numpy
matplotlib
openpyxl
```

Install via:
```bash
pip install opencv-python numpy matplotlib openpyxl
```

---

## 📬 Citation

If you use this code or results in your research, please cite it : 

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- **Dataset**: Berkeley Segmentation Data Set 500 (BSDS500) — Arbelaez et al., IEEE TPAMI 2011
- Evaluation protocol follows standard BSDS500 benchmarking methodology
- Thanks to the UC Berkeley Computer Vision Group for making the dataset publicly available
