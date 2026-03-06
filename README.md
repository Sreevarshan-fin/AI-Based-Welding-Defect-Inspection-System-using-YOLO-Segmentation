# AI-Based-Welding-Defect-Inspection-System-using-YOLO-Segmentation
---

## Project Overview

This project presents an **AI-powered computer vision system for automated welding inspection** using deep learning. The system detects welding defects, segments defect regions, and evaluates weld quality based on defect severity.

The solution uses a **YOLO segmentation model** to identify and segment welding-related features such as cracks, porosity, spatters, and the welding line (weld seam).

The system also calculates **defect density inside the weld seam** and generates a **weld quality score with a PASS / ACCEPTABLE / FAIL decision**, simulating a real industrial quality inspection workflow.

This project is implemented as a **Proof of Concept (POC)** due to limitations in accessing proprietary industrial datasets and factory hardware. However, it demonstrates the feasibility of deploying **AI-based welding inspection systems in manufacturing environments**.

The inspection interface is deployed using **Streamlit**, enabling image, video, and live camera inspection.

---

# Problem Statement

In manufacturing industries such as **automotive, construction, shipbuilding, and pipeline engineering**, welding is widely used to join metal components. The quality of weld joints directly impacts the **structural strength, durability, and safety of the final product**.

During welding processes, defects such as **cracks, porosity, and spatters** may occur. These defects can weaken the weld and lead to:

* Structural failures
* Safety hazards
* Increased maintenance costs
* Product recalls and financial losses

Most manufacturing facilities still rely on **manual visual inspection**, which presents several challenges:

* Human error and inspection inconsistency
* Slow inspection processes
* High labor costs
* Difficulty scaling inspection for high production volumes

To address these challenges, this project develops an **AI-driven welding inspection system** capable of automatically detecting welding defects and evaluating weld quality using deep learning.

---

# Classes Detected

The model detects and segments four classes:

| Class        | Description                                    |
| ------------ | ---------------------------------------------- |
| Crack        | Structural fracture within the weld            |
| Porosity     | Gas pockets trapped during welding             |
| Spatters     | Metal droplets scattered during welding        |
| Welding Line | Weld seam region (inspection region reference) |

The **welding line is used to calculate the weld area**, allowing defect density to be measured within the weld seam rather than the entire image.

---

# Model Architecture

The system uses a **YOLOv8-based segmentation model**.

Base model:

```
yolo26m-seg.pt
```

This architecture enables **real-time object detection with instance segmentation**, making it suitable for industrial inspection systems.

---

# Model Training

### Training Configuration

| Parameter    | Value                      |
| ------------ | -------------------------- |
| Model        | YOLOv8 Medium Segmentation |
| Base Weights | yolo26m-seg.pt             |
| Epochs       | 200                        |
| Batch Size   | 8                          |
| Optimizer    | AdamW                      |
| Task         | Instance Segmentation      |

Training was performed on a labeled welding defect dataset with segmentation masks.

---

# Dataset Summary

| Metric    | Value  |
| --------- | ------ |
| Images    | 586    |
| Instances | 11,382 |
| Classes   | 4      |

Classes include:

* crack
* porosity
* spatters
* welding line

---

# Model Performance

### Overall Model Metrics

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

### Class-wise Performance

| Class        | Instances | Mask mAP50 |
| ------------ | --------- | ---------- |
| Crack        | 336       | 0.822      |
| Porosity     | 3846      | 0.652      |
| Spatters     | 6310      | 0.639      |
| Welding Line | 890       | 0.930      |

The model performs particularly well in detecting **weld seams and cracks**, while smaller defects such as **porosity and spatters** remain more challenging.

---

# Weld Quality Evaluation

The system evaluates weld quality using **defect density within the weld seam**.

### Defect Density

```
Defect Density = Defect Area / Weld Area
```

### Weld Quality Score

```
Quality Score = 100 - Defect Density
```

### Inspection Result

| Defect Density | Result     |
| -------------- | ---------- |
| < 1%           | PASS       |
| 1 – 5%         | ACCEPTABLE |
| > 5%           |   FAIL       |

---

# System Deployment

The system is deployed using **Streamlit**, providing an interactive interface for inspection.

Supported inspection modes:

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
* PASS / ACCEPTABLE / FAIL decision

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

Possible enhancements for real-world deployment:

* Integration with industrial cameras
* Edge AI deployment (Jetson / industrial GPUs)
* Batch inspection for production lines
* Automated defect inspection reports
* Defect heatmap visualization



