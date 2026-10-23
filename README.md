# Leakage-Resistant Mpox Skin Lesion Classification Using Transfer Learning

## 📌 Project Overview

This project develops a binary deep learning classifier to distinguish **Mpox skin lesions from other skin conditions** using the Monkeypox Skin Lesion Dataset (MSLD).

Mpox lesions may resemble chickenpox or measles lesions, making visual classification challenging. This project explores transfer learning with a small dataset while focusing on overfitting prevention, data leakage control, and honest model evaluation.

**Domain:** Healthcare & Medical Imaging  
**Framework:** Python, TensorFlow/Keras  
**CNN Architecture:** MobileNetV2 (ImageNet-pretrained, frozen backbone)

## 🎯 Objectives

- Build a binary image classifier for Mpox vs Others.
- Investigate classification performance with a limited number of original images.
- Prevent augmented copies of validation or test images from entering training.
- Reduce overfitting through transfer learning and regularization.
- Evaluate performance using class-wise metrics and a confusion matrix.
- Discuss the limitations and responsible use of AI in medical imaging.

## 📂 Dataset

**Dataset:** Monkeypox Skin Lesion Dataset (MSLD)

**Source:** [Kaggle — Monkeypox Skin Lesion Dataset](https://www.kaggle.com/datasets/nafin59/monkeypox-skin-lesion-dataset)

The supplied dataset contained 228 image files. One exact duplicate was identified using SHA-256 hashing and removed, leaving **227 unique images**.

### Class Labels

| Class | Label ID |
|---|---:|
| Mpox | 0 |
| Others | 1 |

The Others class represents the non-Mpox images provided and grouped in the dataset.

### Dataset Split

| Split | Mpox | Others | Total |
|---|---:|---:|---:|
| Training | 71 | 87 | 158 |
| Validation | 15 | 19 | 34 |
| Testing | 16 | 19 | 35 |
| **Total** | **102** | **125** | **227** |

The split uses stratified sampling with a fixed random seed of 42.

## ⚙️ Methodology

1. **Data loading:** Load labelled images from the original dataset folders.
2. **Duplicate detection:** Use SHA-256 hashes to identify byte-identical image files.
3. **Data splitting:** Create training, validation, and test sets before augmentation.
4. **Preprocessing:** Resize images to 224 × 224 pixels and apply MobileNetV2 preprocessing.
5. **Data augmentation:** Apply random horizontal flips, mild rotations, and zoom during training only.
6. **Transfer learning:** Use an ImageNet-pretrained MobileNetV2 with a frozen backbone and a binary sigmoid classification head.
7. **Regularization:** Use dropout, L2 regularization, early stopping, and learning-rate reduction.
8. **Evaluation:** Measure test accuracy, precision, recall, F1-score, macro F1, and confusion matrix.

## 🧠 Model Architecture

The classifier consists of:

- MobileNetV2 pretrained on ImageNet.
- Frozen convolutional backbone.
- Global Average Pooling.
- Dropout layer with a rate of 0.4.
- Dense output layer with sigmoid activation.

**Loss function:** Binary cross-entropy  
**Optimizer:** Adam  
**Initial learning rate:** 0.001  
**Input size:** 224 × 224 × 3

Transfer learning reduces the number of trainable parameters compared with training a deep CNN from scratch, making it a reasonable starting point for this small dataset.

## 📊 Experimental Results

The selected model was evaluated on the held-out test set of 35 images.

| Metric | Result |
|---|---:|
| Test accuracy | **82.86%** |
| Macro F1-score | **0.825** |
| Correct predictions | 29 |
| Misclassified images | 6 |

### Per-Class Performance

| Class | Precision | Recall | F1-score |
|---|---:|---:|---:|
| Mpox | 0.86 | 0.75 | 0.80 |
| Others | 0.81 | 0.89 | 0.85 |

### Confusion Matrix

| Actual / Predicted | Mpox | Others |
|---|---:|---:|
| Mpox | 12 | 4 |
| Others | 2 | 17 |

The model correctly classified 12 of 16 Mpox images and 17 of 19 Others images. Four Mpox images were incorrectly classified as Others, highlighting the importance of sensitivity in medical screening applications.

## 🛡️ Overfitting and Data Leakage Prevention

- Original images were split before augmentation.
- Augmentation was applied only during training.
- Exact duplicate files were checked using SHA-256 hashing.
- A fixed random seed was used for reproducibility.
- Validation loss was monitored for early stopping.
- Dropout and L2 regularization were used to reduce overfitting.
- The test set was reserved for final evaluation.

**Limitation:** Exact hashing does not detect every resized, cropped, or recompressed duplicate. Patient-level independence and complete elimination of near-duplicate leakage have not been established.

## ⚠️ Limitations and Responsible Use

- The dataset contains only 227 unique images.
- The test set contains only 35 images, so performance estimates are uncertain.
- The experiment uses one train/validation/test split.
- Generalization across skin tones, image sources, and clinical settings has not been established.
- The model has not undergone external or clinical validation.

This project is intended for educational research. It is **not a clinically validated diagnostic system** and must not replace professional medical assessment or laboratory testing.

## 🚀 Future Improvements

- Perform patient-aware splitting if reliable identifiers become available.
- Investigate near-duplicate images using perceptual hashing.
- Evaluate repeated stratified splits or cross-validation.
- Compare frozen and selectively fine-tuned pretrained models.
- Investigate Grad-CAM explainability and additional robustness tests.
- Validate on an independent, clinically appropriate dataset.

## 🛠️ Technologies Used

- Python
- TensorFlow / Keras
- MobileNetV2
- NumPy and pandas
- Matplotlib and Seaborn
- scikit-learn
- Google Colab
- GitHub

## ▶️ How to Run

1. Clone or download this repository.
2. Open the Jupyter Notebook in Google Colab.
3. Download the dataset from Kaggle and extract it.
4. Update the dataset path to match your folder structure.
5. Run the notebook cells in order.
6. Review the generated plots, metrics, confusion matrix, and misclassified examples.

**Repository:** [Shalini-Thamotharakkannan/Monkeypox](https://github.com/Shalini-Thamotharakkannan/Monkeypox)

## 📚 References

1. Sandler, M. et al. “MobileNetV2: Inverted Residuals and Linear Bottlenecks.” IEEE CVPR, 2018.
2. Srivastava, N. et al. “Dropout: A Simple Way to Prevent Neural Networks from Overfitting.” Journal of Machine Learning Research, 2014.
3. Kingma, D. P. and Ba, J. “Adam: A Method for Stochastic Optimization.” ICLR, 2015.
4. Monkeypox Skin Lesion Dataset (MSLD), Kaggle.
5. Deng, J. et al. “ImageNet: A Large-Scale Hierarchical Image Database.” IEEE CVPR, 2009.

## 🤖 AI Assistance Declaration

AI assistance was used to support code drafting, debugging, and report organization. The student is responsible for verifying the implementation, experimental results, and final submitted work.
