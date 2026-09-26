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

**Dataset Link:**  
https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia

### Dataset Structure

The dataset contains:

- `train`
- `test`
- `val`

The images are divided into:

- `NORMAL`
- `PNEUMONIA`

### Training Dataset

The training dataset contains:

| Category | Number of Images |
|---|---:|
| NORMAL | 1,341 |
| PNEUMONIA | 3,875 |
| **Total** | **5,216** |

For the main experiment, a balanced sample of **100 images** was selected:

- 50 NORMAL images
- 50 PNEUMONIA images

A fixed random seed was used to make the sample selection reproducible.

### Dataset Acquisition

The dataset was downloaded directly into the Google Colab environment using the **Kaggle API** rather than storing the complete dataset in Google Drive.

The Kaggle dataset identifier used in the implementation is:

```text
paultimothymooney/chest-xray-pneumonia
```

The downloaded dataset was extracted into the Colab directory:

```text
/content/chest_xray_dataset/
```

The main dataset directory used for the experiment was:

```text
/content/chest_xray_dataset/chest_xray/
```

---

## 🛠️ Technologies Used

- **Python**
- **Google Colab**
- **OpenCV**
- **NumPy**
- **Pandas**
- **Matplotlib**
- **Kaggle Dataset**

---

## ⚙️ Methodology

The same preprocessing and experimental procedure is applied to both Sobel and Canny.

```text
             X-Ray Image
                  ↓
          Grayscale Conversion
                  ↓
           Resize 512 × 512
                  ↓
            Gaussian Blur
                  ↓
          ┌───────────────┐
          ↓               ↓
        Sobel           Canny
          ↓               ↓
      Edge Map         Edge Map
          └───────┬───────┘
                  ↓
       Quantitative Comparison
                  ↓
          Results & Analysis
```

### Overall Workflow

1. Obtain the X-ray dataset from Kaggle.
2. Explore the dataset structure.
3. Select a balanced sample of 100 images.
4. Convert images to grayscale.
5. Resize images to 512 × 512.
6. Apply Gaussian smoothing.
7. Apply Sobel edge detection.
8. Apply Canny edge detection.
9. Calculate edge density.
10. Measure processing time.
11. Compare Normal and Pneumonia groups.
12. Visualize the results.
13. Perform Canny threshold sensitivity analysis.
14. Interpret the results.

---

## 🔹 Image Preprocessing

Before applying either edge detection method, the X-ray images undergo the same preprocessing steps.

### 1. Grayscale Conversion

The X-ray images are loaded as grayscale images using OpenCV.

Grayscale images contain intensity information in a single channel, which is suitable for edge detection.

### 2. Image Resizing

Each image is resized to:

```text
512 × 512 pixels
```

This provides a common image size for all images and makes the quantitative comparison more consistent.

### 3. Gaussian Blur

A Gaussian filter with a **5 × 5 kernel** is applied:

```python
blurred = cv2.GaussianBlur(resized, (5, 5), 0)
```

Gaussian smoothing helps reduce small-scale noise before edge detection.

The same preprocessing is applied to both Sobel and Canny.

---

## 🔬 Sobel Edge Detection

Sobel is a gradient-based edge detection technique.

In the implementation, two Sobel gradients are calculated:

### Sobel X

Detects intensity changes in one direction.

```python
sobel_x = cv2.Sobel(
    blurred,
    cv2.CV_64F,
    1,
    0,
    ksize=3
)
```

### Sobel Y

Detects intensity changes in the other direction.

```python
sobel_y = cv2.Sobel(
    blurred,
    cv2.CV_64F,
    0,
    1,
    ksize=3
)
```

### Gradient Magnitude

The two gradients are combined to obtain the gradient magnitude.

```python
sobel_magnitude = cv2.magnitude(
    sobel_x.astype(np.float32),
    sobel_y.astype(np.float32)
)
```

The gradient magnitude represents the strength of intensity changes in the image.

For quantitative analysis, a threshold of **50** is applied to the Sobel magnitude to create a binary edge map.

---

## 🔬 Canny Edge Detection

Canny is applied to the same preprocessed X-ray images.

The main experiment uses:

```text
Lower Threshold = 50
Upper Threshold = 150
```

Implementation:

```python
canny_edges = cv2.Canny(
    blurred,
    threshold1=50,
    threshold2=150
)
```

The resulting binary edge map is used for visual and quantitative comparison.

---

## 🧪 Experimental Design

A balanced experimental sample was created from the training dataset.

```text
NORMAL       → 50 images
PNEUMONIA    → 50 images
-------------------------
TOTAL        → 100 images
```

A fixed random seed was used to make the image selection reproducible.

### Measurements Recorded

For every selected X-ray image, the following values were recorded:

- Filename
- Image category
- Sobel edge density
- Canny edge density
- Sobel processing time
- Canny processing time

The measurements were stored in a Pandas DataFrame for further analysis.

---

## 📈 Evaluation Metrics

### Edge Density

Edge density represents the percentage of pixels classified as edges.

It is calculated as:

```text
Edge Density (%) =
(Number of Edge Pixels / Total Number of Pixels) × 100
```

A higher edge density means that a larger proportion of pixels were classified as edges under the selected detection settings.

> **Important:** Edge density is not an accuracy metric.

### Processing Time

Processing time measures how long each edge detection operation takes for an image.

The implementation uses Python's `time.perf_counter()` to measure the execution time.

Processing time is reported in milliseconds.

---

## 📊 Experimental Results

The main experiment was performed on **100 chest X-ray images**:

- 50 NORMAL
- 50 PNEUMONIA

### Overall Results

| Metric | Sobel | Canny |
|---|---:|---:|
| Average Edge Density | **9.480%** | **1.873%** |
| Average Processing Time | **2.229 ms** | **0.992 ms** |

### Result Interpretation

In this particular experimental setup:

- Sobel produced a higher measured average edge density than Canny.
- The measured average processing time was lower for Canny than Sobel.

These measurements correspond specifically to the implementation, image size, parameters and runtime environment used in this project.

They should not be interpreted as universal claims that one algorithm is always faster or more accurate than the other.

---

## 📊 Category-wise Results

The experiment also compares the results separately for NORMAL and PNEUMONIA images.

### Edge Density

| Category | Sobel | Canny |
|---|---:|---:|
| Normal | **11.658%** | **2.345%** |
| Pneumonia | **7.301%** | **1.402%** |

### Processing Time

| Category | Sobel | Canny |
|---|---:|---:|
| Normal | **2.294 ms** | **1.116 ms** |
| Pneumonia | **2.165 ms** | **0.867 ms** |

The category-wise analysis is used to observe how the measured edge characteristics and processing times vary between the two image groups.

The project does **not** use these measurements to classify or diagnose pneumonia.

---

## 🖼️ Visual Analysis

The notebook includes visual comparisons of the edge detection results.

### Sample X-Ray

A sample chest X-ray is displayed before and after preprocessing.

### Sobel Visualization

The following outputs are displayed:

- Sobel X
- Sobel Y
- Sobel Gradient Magnitude

### Canny Visualization

The Canny edge map is displayed using the selected thresholds.

### Side-by-Side Comparison

The project provides a direct visual comparison:

```text
Preprocessed X-Ray
        │
        ├── Sobel Gradient Magnitude
        │
        └── Canny Edge Map
```

This allows the detected boundary patterns produced by the two techniques to be visually examined.

---

## 📊 Graphical Analysis

The implementation includes graphical analysis of the experimental results.

### 1. Average Edge Density

A graph compares the average edge density of Sobel and Canny.

Observed values:

```text
Sobel  → 9.480%
Canny  → 1.873%
```

### 2. Average Processing Time

A graph compares the average processing time of both methods.

Observed values:

```text
Sobel  → 2.229 ms
Canny  → 0.992 ms
```

### 3. Category-wise Edge Density

The category-wise analysis compares edge density for:

- NORMAL images
- PNEUMONIA images

for both Sobel and Canny.

### 4. Category-wise Processing Time

Processing time is also compared separately for NORMAL and PNEUMONIA images.

---

## 🔧 Canny Threshold Sensitivity Analysis

An additional experiment was performed to study how Canny threshold selection affects edge detection.

The following threshold pairs were tested:

| Low Threshold | High Threshold |
|---:|---:|
| 30 | 90 |
| 50 | 150 |
| 70 | 210 |
| 100 | 200 |

For every threshold pair:

1. Canny edge detection is performed.
2. Edge pixels are counted.
3. Edge density is calculated.
4. The resulting edge map is visually examined.

This analysis demonstrates that changing the Canny thresholds can change the resulting detected edge structure.

---

## 🔍 Results Summary

The experiment provides both visual and quantitative comparison between Sobel and Canny.

### Main Observations

- Both methods can detect intensity boundaries in chest X-ray images.
- Sobel provides horizontal and vertical gradient information and a combined gradient magnitude.
- Canny produces a binary edge map using its edge-detection pipeline and selected thresholds.
- The measured edge density was higher for Sobel in this experiment.
- The measured processing time was lower for Canny in this experiment.
- Canny's output changes when different threshold pairs are used.
- Results depend on preprocessing, image resolution, threshold parameters and execution environment.

---

## ⚠️ Limitations

- The main experiment uses a balanced sample of 100 images rather than the complete dataset.
- Ground-truth boundary masks were not available for calculating boundary-detection accuracy.
- Edge density is not an accuracy measurement.
- Sobel edge density depends on the threshold used to convert the gradient magnitude into a binary edge map.
- Canny results depend on the selected threshold values.
- Processing time depends on the implementation, image size and runtime environment.
- The project performs edge/boundary detection and does not diagnose pneumonia.

---

## 🚀 Future Scope

Possible extensions include:

- Using a larger experimental sample.
- Using ground-truth boundary annotations.
- Evaluating additional edge detection algorithms.
- Performing parameter optimization.
- Conducting more extensive computational benchmarking.
- Comparing different preprocessing techniques.
- Evaluating the methods on other medical imaging datasets.
- Investigating automated selection of Canny thresholds.

---

## 📁 Repository Structure

```text
Sobel-vs-Canny-Medical-XRay/
│
├── README.md
├── Sobel_vs_Canny_Medical_XRay_Boundary_Detection.ipynb
└── requirements.txt
```

### Files

**README.md**

Contains the project overview, methodology, results and documentation.

**Sobel_vs_Canny_Medical_XRay_Boundary_Detection.ipynb**

Contains the complete implementation, dataset processing, experiments, visualizations and analysis.

**requirements.txt**

Contains the main Python libraries required for the project.

---

## 📓 Notebook Structure

The implementation notebook is organized into the following sections:

1. Introduction
2. Problem Statement
3. Objectives
4. Dataset
5. Tools & Technologies
6. Methodology
7. Environment Setup
8. Dataset Loading & Exploration
9. Experimental Design
10. Preliminary Demonstration
11. Main Experiment
12. Quantitative Results
13. Visual Results
14. Canny Threshold Sensitivity
15. Results & Discussion
16. Limitations
17. Conclusion
18. Future Scope

---

## 📚 Dataset Reference

**Chest X-Ray Images (Pneumonia)**

Kaggle Dataset:

https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia

Dataset identifier:

```text
paultimothymooney/chest-xray-pneumonia
```

---

## 👩‍💻 Project Information

**Project Title:**  
Comparative Study of Sobel vs. Canny for Medical X-Ray Boundary Detection

**Course:**  
Computer Vision

**Unit:**  
Unit 1 – Edge Detection

**Case Design Topic:**  
Topic 5

---

## ⭐ Key Note

This project is a **comparative edge-detection study**.

It should not be interpreted as a medical diagnostic system or a pneumonia classification model.

The results reported in this repository represent the observations obtained from the specific experimental setup used in the project.
