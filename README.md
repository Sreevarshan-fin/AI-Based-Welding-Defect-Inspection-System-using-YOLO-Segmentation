
# Welding Defect Inspection using YOLO Segmentation

## Project Overview

This project presents a **computer vision–based welding inspection system** that uses YOLO segmentation to detect welding defects and evaluate weld quality.

The model identifies and segments key welding-related features, including **cracks, porosity, spatters, and the welding line (weld seam)**. Using these segmented regions, the system calculates the **defect density within the weld seam**, allowing the inspection to focus specifically on the critical weld region rather than the entire image.

Based on the calculated defect density, the system generates an **automated weld quality score** and classifies the weld into one of three categories:

* **PASS**
* **ACCEPTABLE**
* **FAIL**

To demonstrate the full inspection workflow, the system is deployed using **Streamlit**, providing an interactive interface that supports **image upload, video inspection, and live camera analysis**.

---

# Problem Statement

Welding is widely used across manufacturing industries such as **automotive, construction, shipbuilding, and pipeline engineering** to join metal components. The quality of weld joints directly affects the **structural strength, durability, and safety** of the final product.

During welding processes, several types of defects may occur, including:

* Cracks
* Porosity
* Spatters

These defects can significantly weaken weld joints and may result in:

* Structural failures
* Safety hazards
* Increased maintenance costs
* Product recalls and financial losses

Despite these risks, many manufacturing facilities still rely on **manual visual inspection**, which presents several limitations:

* Human error and inconsistent inspection results
* Slow inspection processes
* High labor costs
* Difficulty scaling inspection for high-volume production lines

To address these challenges, this project demonstrates a **computer vision–based automated welding inspection system** capable of detecting defects and evaluating weld quality.

---

# Classes Detected

The segmentation model detects **four classes** related to welding inspection.

| Class        | Description                                       |
| ------------ | ------------------------------------------------- |
| Crack        | Structural fracture within the weld               |
| Porosity     | Gas pockets trapped during welding                |
| Spatters     | Metal droplets scattered during welding           |
| Welding Line | Weld seam region used as the inspection reference |

The **welding line** is particularly important because it defines the **weld region**, allowing defect density to be calculated specifically inside the weld seam.

---

# Model Architecture

The system uses a **YOLOv8-based instance segmentation model**.

Base Model:

```
yolo26m-seg.pt
```

YOLO segmentation enables the system to perform **real-time object detection and pixel-level segmentation**, which makes it suitable for industrial inspection tasks where identifying precise defect regions is important.

---

# Model Training

## Training Configuration

| Parameter    | Value                      |
| ------------ | -------------------------- |
| Model        | YOLOv8 Medium Segmentation |
| Base Weights | yolo26m-seg.pt             |
| Epochs       | 200                        |
| Batch Size   | 8                          |
| Optimizer    | AdamW                      |
| Task         | Instance Segmentation      |

The model was trained on a labeled welding defect dataset containing segmentation masks for each defect class.

---

# Dataset Summary

| Metric    | Value  |
| --------- | ------ |
| Images    | 586    |
| Instances | 11,382 |
| Classes   | 4      |

Detected classes include:

* crack
* porosity
* spatters
* welding line

---

# Model Performance

## Overall Model Metrics

| Metric         | Value |
| -------------- | ----- |
| Box Precision  | 0.821 |
| Box Recall     | 0.712 |
| Box mAP50      | 0.776 |
| Box mAP50-95   | 0.429 |
| Mask Precision | 0.807 |
| Mask Recall    | 0.701 |
| Mask mAP50     | 0.761 |
| Mask mAP50-95  | 0.442 |

---

## Class-wise Performance

| Class        | Instances | Mask mAP50 |
| ------------ | --------- | ---------- |
| Crack        | 336       | 0.822      |
| Porosity     | 3846      | 0.652      |
| Spatters     | 6310      | 0.639      |
| Welding Line | 890       | 0.930      |

The model performs particularly well in detecting **weld seams and cracks**, while smaller defects such as **porosity and spatters remain more challenging**.

---

# Weld Quality Evaluation

The system evaluates weld quality by measuring **defect density within the weld seam**.

## Defect Density Formula

```
Defect Density = Defect Area / Weld Area
```

## Quality Score Calculation

```
Quality Score = 100 - Defect Density
```

## Inspection Result Criteria

| Defect Density | Result     |
| -------------- | ---------- |
| < 1%           | PASS       |
| 1 – 5%         | ACCEPTABLE |
| > 5%           | FAIL       |

This approach simulates a **basic automated quality inspection rule used in industrial inspection workflows**.

---

# System Deployment

The system is deployed using **Streamlit**, providing an interactive inspection interface.

Supported inspection modes include:

* Image upload
* Video upload
* Live camera inspection

The interface displays:

* Original image
* Segmentation output
* Detected defects
* Weld area and defect area
* Defect density
* Quality score
* Final inspection result (PASS / ACCEPTABLE / FAIL)

---

# Tech Stack

* Python
* PyTorch
* Ultralytics YOLO
* OpenCV
* Streamlit
* NumPy
* PIL

---

# Inspection Pipeline

```
Input Image / Video / Camera
        ↓
YOLO Segmentation Model
        ↓
Defect Detection
        ↓
Segmentation Masks
        ↓
Weld Area Calculation
        ↓
Defect Density Calculation
        ↓
Quality Score Generation
        ↓
Inspection Result (PASS / FAIL)
```

---

# Future Improvements

Possible enhancements for real-world deployment include:

* Integration with industrial cameras
* Edge AI deployment using Jetson or industrial GPUs
* Batch inspection for production lines
* Automated inspection reports
* Defect heatmap visualization

---

