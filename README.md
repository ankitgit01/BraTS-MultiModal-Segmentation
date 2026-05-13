# 3D Brain Tumor Segmentation using Attention U-Net

## Overview
This repository provides an end-to-end pipeline for the automated segmentation of 3D brain tumors from multi-modal MRI scans. Leveraging deep learning techniques specifically designed for volumetric medical imaging, this project accurately isolates tumorous regions to aid in medical analysis and diagnosis. 

The pipeline includes automated data downloading, rigorous NIfTI preprocessing, disk caching for accelerated training, and an advanced 3D Attention U-Net model with Test-Time Augmentation (TTA).

## Repository Contents
* **`BraTS_seg.ipynb`**: The primary Jupyter Notebook containing the full execution pipeline (data ingestion, preprocessing, model definition, training loop, and evaluation).
* **`Applied_ML_Brain_Tumor_Segmentation_Report.docx`**: A detailed project report discussing the methodology, experiments, theoretical background, and comprehensive analysis of the results.
* **`requirements.txt`**: A list of Python dependencies required to execute the project successfully.

## Dataset & Preprocessing
* **Source Dataset**: BraTS 2021 Task 1 Dataset
* **Modalities Used**: T1-weighted contrast-enhanced (T1CE) and Fluid-attenuated inversion recovery (FLAIR).
* **Format**: 3D NIfTI volumes (`.nii.gz`).
* **Preprocessing Pipeline**:
  * **Normalization**: Voxel intensities are normalized per volume.
  * **Patch Extraction**: Volumes are cropped into smaller 64x64x64 patches (center-cropped or random-cropped).
  * **Disk Caching**: To overcome disk I/O bottlenecks during training, NIfTI files are decompressed and saved as highly optimized, compressed NumPy arrays (`.npz` and `.npy`).

## Model Architecture
The core model is a **3D Attention U-Net** enhanced with several custom mechanisms to improve spatial feature representation and reduce overfitting:
* **Squeeze-and-Excitation (SE) Blocks**: Integrated within double-convolution stages for channel-wise feature recalibration, allowing the network to emphasize informative channels.
* **Attention Gates**: Applied in the decoder stages to spatially filter features propagated through skip connections, focusing the model on target structures while suppressing irrelevant background noise.
* **Dropout Regularization**: Strategically placed Dropout layers (p=0.10) to ensure a lightweight but robust regularization against overfitting.

## Loss Function
To combat the severe class imbalance typical in medical image segmentation (where tumor pixels are sparse compared to background pixels), the project uses a **Combined Objective Function**:
* **Dice Loss** (Weighted at 0.6): Optimizes spatial overlap.
* **Focal Loss** (Weighted at 0.4): Down-weights easily classified examples to focus the network on harder-to-segment boundaries.

## Advanced Evaluation Metrics & Techniques
The model's performance is rigorously measured across multiple dimensions. The standard evaluation metrics include:
* **Dice Coefficient**
* **Intersection over Union (IoU)**
* **Precision and Recall**
* **95th-percentile Hausdorff Distance (HD95)**

### Enhancements during Evaluation
1. **Test-Time Augmentation (TTA)**: Evaluates predictions across 8 different geometric views (flips across H, W, D axes) and averages the predictions to improve the stability and robustness of the final tumor mask. 
2. **Gradient-Based Modality Importance**: A custom analysis routine utilizing gradient attributions to determine which MRI modality (T1CE vs. FLAIR) contributes the most to the network's predictive performance.

## How to Run

### Recommended Environment
It is highly recommended to run this project in a Cloud Jupyter environment like **Google Colab** or **Kaggle** due to the heavy computational requirements of 3D CNNs.

### Steps
1. Clone the repository and install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Open BraTS_seg.ipynb in your Jupyter environment.
    - Hardware Requirement: Ensure you enable a GPU backend (e.g., GPU T4 or GPU T4 ×2).

3. Run the notebook cells sequentially.

Note: The dataset extraction, downloading, and patch caching steps are fully automated in the notebook and will execute on the first run.
