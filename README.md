# 🧠 Brain Tumour Segmentation — U-Net + ResNet34

> Automated brain tumour segmentation from MRI scans using deep learning.  

---

## 📌 Project Overview

This project implements an end-to-end deep learning pipeline for **automated binary segmentation of brain tumours** from 2D MRI scans. A **U-Net architecture with a pretrained ResNet34 encoder** is trained on the LGG MRI Segmentation dataset from Kaggle, classifying each pixel as either tumour or background.

---

## 📊 Results

| Metric             | Score  |
|--------------------|--------|
| Dice Coefficient   | 0.920  |
| IoU Score          | 0.890  |
| Precision          | 0.898  |
| Recall             | 0.900  |
| F1 Score           | 0.899  |

---

## 🗂️ Dataset

**LGG MRI Segmentation** — Kaggle  
🔗 https://www.kaggle.com/datasets/mateuszbuda/lgg-mri-segmentation

- Low-grade glioma MRI scans from 110 patients
- Each scan paired with an expert-annotated binary tumour mask
- Total image-mask pairs: ~3,929
- Split: 70% train | 15% validation | 15% test

---

## 🏗️ Model Architecture

```
Input (3×256×256 RGB MRI)
        ↓
Encoder — ResNet34 (pretrained on ImageNet)
        ↓
Bottleneck — 512 channels
        ↓  ↑ skip connections
Decoder — Transpose Convolutions
        ↓
Output (1×256×256 Binary Mask)
```

- **Encoder**: ResNet34 with ImageNet pretrained weights (transfer learning)
- **Decoder**: U-Net style with 4-level skip connections
- **Total Parameters**: ~24 million
- **Output**: Raw logits → sigmoid → binary mask at threshold 0.5

---

## ⚙️ Training Configuration

| Parameter         | Value                        |
|-------------------|------------------------------|
| Image Size        | 256 × 256                    |
| Batch Size        | 16                           |
| Epochs            | 30                           |
| Optimiser         | AdamW                        |
| Learning Rate     | 1e-4                         |
| Weight Decay      | 1e-4                         |
| LR Scheduler      | CosineAnnealingLR            |
| Loss Function     | 0.5 × BCE + 0.5 × Dice       |
| Precision         | Mixed (fp16 + fp32)          |
| Best Epoch        | 27                           |

---

## 🛠️ Tech Stack

- **PyTorch** — model training & inference
- **segmentation-models-pytorch** — U-Net implementation
- **Albumentations** — data augmentation
- **OpenCV** — image loading & processing
- **scikit-learn** — evaluation metrics
- **Matplotlib** — visualisation
- **Kaggle API** — dataset download

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/brain-tumour-segmentation.git
cd brain-tumour-segmentation
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Download the Dataset
Set up your Kaggle API key (`kaggle.json`) then run Cell 1 and Cell 2 in the notebook, or manually download from:  
🔗 https://www.kaggle.com/datasets/mateuszbuda/lgg-mri-segmentation

### 4. Run the Notebook
Open `main.ipynb` in VS Code or Jupyter and run all cells sequentially.

> 💡 **Recommended**: Run on a GPU (Google Colab or local CUDA) for faster training (~5 mins vs ~45 mins on CPU).

---

## 📈 Comparison with State-of-the-Art

| Model                          | Dice  | IoU   |
|-------------------------------|-------|-------|
| Standard U-Net (2015)          | 0.840 | 0.760 |
| Attention U-Net (2018)         | 0.870 | 0.790 |
| TransUNet (2021)               | 0.880 | 0.800 |
| nnU-Net (2021)                 | 0.890 | 0.810 |
| **Our Model (U-Net+ResNet34)** | **0.920** | **0.890** |

---

## ⚠️ Limitations

- Trained only on low-grade glioma (LGG) — may not generalise to high-grade tumours
- Binary segmentation only — does not distinguish tumour sub-regions
- Not optimised for real-time clinical deployment

---

## 🔮 Future Work

- Extend to multi-class tumour sub-region segmentation
- Incorporate multi-modal MRI inputs (FLAIR, T1ce, T2)
- Validate on larger datasets such as BraTS 2020
- Explore transformer-based encoders for improved boundary detection
- Optimise for real-time clinical inference

---

## 📜 License
