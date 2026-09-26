# Comparative Study of Sobel vs. Canny for Medical X-Ray Boundary Detection

## 📌 Project Overview

This project presents a comparative study of two classical edge detection techniques — **Sobel** and **Canny** — for detecting boundaries in medical chest X-ray images.

The same preprocessing pipeline is applied to both methods, and their results are compared using visual analysis and quantitative measurements.

> **Note:** This project focuses on edge/boundary detection. It is not a pneumonia diagnosis or classification model.

---

## 🎯 Objectives

- Study Sobel edge detection.
- Study Canny edge detection.
- Apply both methods to medical chest X-ray images.
- Compare their detected edge patterns visually.
- Compare edge density quantitatively.
- Compare processing time.
- Analyze the effect of Canny threshold selection.

---

## 📊 Dataset

The project uses the **Chest X-Ray Images (Pneumonia)** dataset from Kaggle.

Dataset:

https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia

The dataset contains:

- NORMAL X-rays
- PNEUMONIA X-rays
- Training, testing and validation folders

For the main experiment, a balanced sample of:

- 50 NORMAL images
- 50 PNEUMONIA images

was selected from the training data.

A fixed random seed was used to make the selection reproducible.

---

## ⚙️ Methodology

The same preprocessing pipeline is applied to every image:

```text
X-Ray Image
     ↓
Grayscale
     ↓
Resize to 512 × 512
     ↓
Gaussian Blur
     ↓
 ┌───────────────┐
 ↓               ↓
Sobel           Canny
 ↓               ↓
Edge Map        Edge Map
 └───────┬───────┘
         ↓
Quantitative Comparison
         ↓
Results & Analysis
