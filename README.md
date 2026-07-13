# 🔍 Real-Time Weapon Detection: YOLOv8 vs YOLOv11

**MSc Artificial Intelligence Dissertation — University of Salford (2025)**  
*Supervisor: Dr. Ali Alameer | Grade: Distinction (70%)*

---

## Overview

This project presents a **comparative study of YOLOv8 and YOLOv11** for real-time weapon detection in smart surveillance systems. Using a custom 9-class dataset of 5,678 annotated images, both models were trained, evaluated, and compared across accuracy, recall, and inference speed — providing evidence-based deployment recommendations for security-critical environments.

> One of the first comparative evaluations of YOLOv11 on a real-world weapon detection task.

---

## Results Summary

| Model | mAP@50 | mAP@50-95 | Precision | Recall | Inference Speed |
|-------|--------|-----------|-----------|--------|-----------------|
| YOLOv8n | 86.59% | 73.03% | 85.84% | 80.23% | **2.59ms (386 FPS)** |
| YOLOv11s | **89.21%** | **75.41%** | 85.39% | **84.34%** | 5.70ms (175 FPS) |

**Key finding:** YOLOv11 achieves higher accuracy and recall (catching ~4% more weapons), while YOLOv8 runs 2.2× faster — making each better suited to different deployment contexts.

---

## Dataset

- **Total images:** 5,678 (Train: 4,848 | Val: 443 | Test: 387)
- **Classes (9):** Gunmen, Rifle, Blunt Object, Knife, Knife Attacker, Person, Pistol, Shotgun, Submachine Gun
- **Sources:** Open Images v6, CCTV-style security footage, stock photography
- **Annotations:** YOLO format (.txt bounding boxes)
- **Challenges addressed:** Significant class imbalance (blunt objects dominant; submachine guns rare)

---

## Methodology

This project follows the **CRISP-DM** framework:

```
Business Understanding → Data Understanding → Data Preparation
→ Modelling → Evaluation → Deployment Considerations
```

**Data augmentation strategy:**
- Mosaic augmentation (4-image composite, enabled for epochs 1–20)
- Horizontal flips (p=0.5)
- Scale variation (±20%)
- HSV colour jitter (hue ±1.5%, saturation ±70%, value ±40%)
- Gaussian blur (p=0.01)

---

## Technical Stack

| Component | Detail |
|-----------|--------|
| Language | Python 3.10 |
| Framework | PyTorch 2.1.2 + Ultralytics YOLO 8.3.181 |
| Hardware | NVIDIA Tesla P100 GPU (16GB VRAM) via Kaggle |
| Training | 30 epochs, batch 16, image size 640×640, SGD + cosine LR |
| Export | ONNX (for deployment) |

---

## Model Architecture Comparison

| Feature | YOLOv8n | YOLOv11s |
|---------|---------|----------|
| Parameters | 3.0M | 9.4M |
| Layers | 168 | 100 |
| Key modules | CSP-Darknet53, anchor-free head | C3k2, C2PSA (spatial attention), SPPF |
| Strength | Speed + efficiency | Recall + small object detection |

---

## Key Findings

**YOLOv11 outperformed YOLOv8 in 7 of 9 weapon classes**, with the most notable gains in:
- **Submachine Gun:** +5.5 AP50, +10% recall
- **Shotgun:** +5.2 AP50, +10.7% recall  
- **Knife:** +4.9 AP50, +13.9% recall
- **Person (unarmed):** +17 AP50, +16% recall — critical for reducing false alarms

**YOLOv8 retained the edge** for Rifle detection (96.1% vs 87.4% precision) and overall speed.

---

## Deployment Recommendations

| Scenario | Recommended Model | Reason |
|----------|------------------|--------|
| High-security (airports, schools) | **YOLOv11s** | Higher recall — fewer missed weapons |
| Large-scale CCTV networks | **YOLOv8n** | 2.2× faster — handles more streams |
| Edge devices (Jetson Nano) | **YOLOv8n** | YOLOv11 may drop to ~5 FPS on CPU |

---

## Repository Structure

```
weapon-detection-yolov8-vs-yolov11/
│
├── weapon-detection-yolov8-and-yolov11.ipynb   # Full training & evaluation notebook
├── requirements.txt                              # Python dependencies
├── README.md                                     # This file
└── .gitignore
```

> **Note:** The dataset is not included in this repository due to size. It is available on Kaggle: [weapon-detection dataset](https://www.kaggle.com/datasets)

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/Kafi001/weapon-detection-yolov8-vs-yolov11.git
cd weapon-detection-yolov8-vs-yolov11
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run on Kaggle (recommended)
The notebook is designed to run on **Kaggle with GPU enabled** (Tesla P100).  
Upload the notebook to Kaggle and attach the weapon-detection dataset as input.

### 4. Run locally
If running locally, update the `SRC` path in the notebook to point to your local dataset directory.

---

## Evaluation Metrics Used

- **mAP@50** — Mean Average Precision at IoU threshold 0.50 (PASCAL VOC standard)
- **mAP@50-95** — COCO-style AP averaged across IoU thresholds 0.50–0.95
- **Precision** — Of all detections made, how many were correct
- **Recall** — Of all real weapons, how many were detected
- **Inference speed** — Milliseconds per image (preprocessing + NMS included)

---

## Citation

If you use this work, please cite:

```
Al Kafi, A. (2025). Smart Surveillance System for Real-Time Threat Detection 
Using YOLOv8 and YOLOv11. MSc Dissertation, University of Salford, Manchester, UK.
```

---

## Author

**Abdullah Al Kafi**  
MSc Artificial Intelligence, University of Salford (2025)  
[LinkedIn](https://www.linkedin.com/in/abdullah-al-kafi-173b4325b) | [GitHub](https://github.com/Kafi001)
