# BraTS-MultiModal-Segmentation

Multi-modal 3D brain tumor segmentation on the BraTS 2021 dataset using an improved Attention Residual UNet architecture with deep supervision, focal loss, test-time augmentation, and robustness evaluation.

---

## Features

- Multi-modal MRI segmentation (T1, T1CE, T2, FLAIR)
- 3D Attention Residual UNet architecture
- Deep supervision for stable training
- Focal Loss + Dice-based optimization
- Test-Time Augmentation (TTA)
- Modality importance analysis
- Gaussian noise robustness testing
- Hausdorff Distance (HD95) evaluation
- Mixed precision training
- Numpy caching for faster preprocessing

---

## Dataset

Dataset used:
- BraTS 2021 Brain Tumor Segmentation Challenge

Required modalities per patient:
- T1
- T1CE
- T2
- FLAIR

Ground truth:
- Segmentation mask

Expected dataset structure:

```text
BraTS2021/
├── BraTS2021_00000/
│   ├── *_t1.nii.gz
│   ├── *_t1ce.nii.gz
│   ├── *_t2.nii.gz
│   ├── *_flair.nii.gz
│   └── *_seg.nii.gz
```

---

## Model Architecture

The project uses an enhanced 3D UNet-based segmentation model with:

- Residual convolution blocks
- Attention gates
- Encoder-decoder structure
- Skip connections
- Deep supervision outputs

Input:
- 4-channel 3D MRI volume

Output:
- Multi-class tumor segmentation mask

---

## Training Pipeline

### Preprocessing

- MRI volume loading
- Intensity normalization
- Volume resizing/cropping
- Numpy caching

### Training

- Mixed precision training
- OneCycleLR scheduler
- Data augmentation
- Deep supervision loss aggregation

### Validation

Metrics used:
- Dice Score
- Hausdorff Distance 95 (HD95)

---

## Test-Time Augmentation

The model performs inference using multiple spatial transformations and merges predictions to improve robustness and segmentation quality.

Implemented TTA combinations include:
- Original
- Horizontal flip
- Vertical flip
- Depth flip
- Combined flips

---

## Modality Importance Analysis

Each MRI modality is evaluated independently to estimate its contribution to segmentation performance.

The analysis helps identify:
- Most informative modality
- Redundancy between modalities
- Model dependence on specific MRI sequences

---

## Robustness Evaluation

Robustness testing is performed by injecting Gaussian noise into validation samples and re-evaluating segmentation performance.

This measures:
- Noise sensitivity
- Generalization stability
- Real-world robustness

---

## Installation

Clone the repository:

```bash
git clone https://github.com/username/BraTS-MultiModal-Segmentation.git
cd BraTS-MultiModal-Segmentation
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Requirements

Main libraries:
- Python 3.10+
- PyTorch
- MONAI
- NumPy
- nibabel
- scikit-image
- matplotlib
- tqdm

---

## Usage

### Train

```bash
python train.py
```

### Validate

```bash
python validate.py
```

### Run Inference

```bash
python inference.py
```

---

## Results

The model is evaluated using:
- Dice Score
- HD95
- Robustness under Gaussian noise
- TTA-enhanced predictions

---

## Project Structure

```text
├── data/
├── cache/
├── models/
├── notebooks/
├── train.py
├── validate.py
├── inference.py
├── requirements.txt
└── README.md
```

---

## Future Improvements

- Transformer-based encoder integration
- Cross-validation training
- Lightweight deployment version
- Automated hyperparameter tuning
- Clinical visualization dashboard

---

## License

This project is intended for academic and research purposes.
