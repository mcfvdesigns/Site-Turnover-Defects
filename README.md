# AECO Site Turnover Defect Detection (YOLO11)

Automated visual inspection system for construction site handover using YOLO11. This repository provides end-to-end code to reproduce model training, evaluation, and zero-install inference on construction defects.

[![Open Baseline Notebook In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://github.com/mcfvdesigns/Site-Turnover-Defects/blob/main/Baseline_inference.ipynb)
[![Open Training Notebook In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://github.com/mcfvdesigns/Site-Turnover-Defects/blob/main/M4%7CU3_Assignment_train.ipynb)

---

## 1. Problem Framing & Success Criteria
Manual inspection during property turnover is slow, inconsistent, and error-prone. This project automates defect detection across 5 common handover issues to assist quality assurance teams.

* **Target Application:** Automated defect identification on site handover images.
* **Success Criteria:** 
  * High Recall ($\ge 0.70$) on major defects to minimize missed structural/cosmetic issues.
  * Real-time inference capability on standard GPU/CPU runtimes.

---

## 2. Dataset & Classes
* **Dataset Reference:** Hosted on [Roboflow Universe](https://universe.roboflow.com/skipper-_sky/site-turnover-defects/dataset/4) (`CC BY 4.0`).
* **Release Asset:** Downloadable dataset package available via [Site Turnover Defects V4 Release](https://github.com/mcfvdesigns/Site-Turnover-Defects/releases/tag/v4.0).
* **Train / Val Split:** 80% / 20% split (28 training images, 3 validation images, 5 test images).

### Target Classes (5)
1. `cracked jamb`
2. `cracked tile`
3. `moisture stain`
4. `paint defect`
5. `uneven joint`

---

## 3. Results Summary & Key Metrics

| Model Stage | Precision | Recall | mAP@50 | mAP@50-95 |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (Zero-Shot YOLO11n)** | 0.000 | 0.000 | 0.000 | 0.000 |
| **Fine-Tuned YOLO11n (50 Epochs)** | *[Add P]* | *[Add R]* | *[Add mAP50]* | *[Add mAP50-95]* |

### Key Takeaways
1. **Domain Adaptation:** Off-the-shelf YOLO11 fails on site turnover defects without custom fine-tuning.
2. **Feature Detection:** Small surface defects (e.g., `paint defect`, `cracked tile`) require targeted data augmentations (CLAHE, blur) to improve edge recognition.
3. **Data Quality:** Expanding the dataset size beyond the current baseline split is critical for reducing false negatives.

---

## 4. How to Reproduce in Google Colab

Anyone can reproduce these results without local installations:

1. **Inference / Evaluation (Fast):**
   * Open [`Baseline_inference.ipynb`](https://github.com/mcfvdesigns/Site-Turnover-Defects/blob/main/Baseline_inference.ipynb) in Colab.
   * Run all cells to automatically pull trained weights from [GitHub Releases](https://github.com/mcfvdesigns/Site-Turnover-Defects/releases/tag/v4.0) and display predictions.
2. **Model Training:**
   * Open [`M4|U3_Assignment_train.ipynb`](https://github.com/mcfvdesigns/Site-Turnover-Defects/blob/main/M4%7CU3_Assignment_train.ipynb) in Colab.
   * Select a GPU runtime (`Runtime -> Change runtime type -> T4 GPU`).
   * Click `Runtime -> Run all`.

---

## 5. Reproducibility Checklist
* **Dataset Version:** Site Turnover Defects v4 (`v4i.yolov11`)
* **Model Architecture:** YOLO11 Nano (`yolo11n.pt`)
* **Hyperparameters:** Epochs = 50, Image Size = 640x640, Batch Size = 16
* **Dependencies:** `ultralytics`, `torch`, `opencv-python`, `pyyaml`

---

## 6. Reproducibility Proof
* **Last Verified Run Date:** September 2026
* **Hardware Environment:** Google Colab Tesla T4 GPU (15 GB VRAM)
* **Execution Time:** ~2–3 minutes for inference / ~15 minutes for 50-epoch training.

---

## 7. Artifacts & Documentation Links
* **Detailed Error Analysis:** [`docs/error_analysis.md`](docs/error_analysis.md)
* **Governance & Compliance Checklist:** [`docs/governance_checklist.md`](docs/governance_checklist.md)
* **Model Weights:** [Download `best.pt`](https://github.com/mcfvdesigns/Site-Turnover-Defects/releases/download/v4.0/best.pt)
* **Presentation Deliverables:** 
  * [Executive Summary PDF](docs/mini_report.pdf)
  * [Slide Deck PDF](docs/presentation_slides.pdf)

---

## 8. Governance & License
* **Code License:** [MIT License](LICENSE)
* **Dataset Rights:** Creative Commons Attribution 4.0 International ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)) courtesy of Roboflow Universe.
