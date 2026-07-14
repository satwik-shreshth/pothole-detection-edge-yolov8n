![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![Research Project](https://img.shields.io/badge/Type-Research%20Project-brightgreen.svg)
![Platform](https://img.shields.io/badge/Platform-Edge%20%2F%20Raspberry%20Pi-red.svg)
![Status](https://img.shields.io/badge/Status-Stable-success.svg)
![Model](https://img.shields.io/badge/Model-YOLOv8n-orange.svg)
![Cite](https://img.shields.io/badge/Cite-This%20Work-blue.svg)

---

#  Pothole Detection on the Edge — YOLOv8n
### Fine-Tuned Real-Time Pothole Detector with Cross-Format CPU Benchmarking for Embedded Deployment

---

##  Project Overview

This project fine-tunes **YOLOv8n** for real-time pothole detection and evaluates its readiness for deployment on resource-constrained edge hardware such as a Raspberry Pi–class robot.

Beyond training a single model, the core contribution of this work is a **CPU-only benchmark across three deployment formats** — native **PyTorch**, exported **ONNX**, and exported **NCNN** — to quantify the real trade-off between inference latency and detection accuracy when a model leaves the GPU-equipped training environment and has to run on embedded, CPU-only hardware in the field.

The pipeline, from dataset preparation through training, export, and benchmarking, is documented end-to-end in `pothole-detection-for-robot.ipynb`, with the full methodology and results written up in `Report.pdf`.

---

##  Aim of the Project

The primary aim of this project is to design and evaluate a **lightweight, edge-deployable pothole detection system** that:

- Fine-tunes YOLOv8n on a pothole-specific dataset for single-class object detection
- Exports the trained model to multiple inference-ready formats (PyTorch, ONNX, NCNN)
- Benchmarks CPU-only inference latency and accuracy across all three formats
- Identifies the most practical export format for real-time deployment on embedded/robotic hardware
- Produces reproducible training metrics, plots, and qualitative detection samples
- Establishes a baseline pipeline that can be extended toward autonomous robot navigation around road damage

---

##  System / Pipeline Architecture

The project is organized into **four stages**:

### 1️⃣ Data Preparation
- Pothole detection dataset configured via `data.yaml` (class definitions, train/val paths)
- Preprocessing and augmentation handled through the Ultralytics YOLOv8 training pipeline

### 2️⃣ Model Training
- **Architecture:** YOLOv8n (Ultralytics), fine-tuned for single-class pothole detection
- Training and validation carried out in `pothole-detection-for-robot.ipynb`
- Checkpoints saved as `best.pt` (best validation performance) and `last.pt` (final epoch)
- Training/validation metrics logged to `results.csv` and `results.json`, visualized in `plots/`

### 3️⃣ Multi-Format Export
- **PyTorch (`best.pt`):** native baseline, GPU/CPU capable
- **ONNX (`best.onnx`):** cross-platform, hardware-agnostic inference graph
- **NCNN (`best_ncnn_model/`):** ARM/embedded-CPU-optimized inference, targeting Raspberry Pi–class deployment

### 4️⃣ CPU Benchmarking & Evaluation
- Controlled, CPU-only inference benchmark across all three export formats
- Comparison of inference latency, FPS, and detection accuracy per format
- Qualitative detection samples captured in `sample_detections/`
- Full analysis and discussion in `Report.pdf`

---

## 📂 Repository Structure

```text
pothole-detection-edge-yolov8n/
│
├── pothole-detection-for-robot.ipynb   # End-to-end training, export, and benchmarking notebook
├── data.yaml                           # Dataset config (classes, paths) for YOLOv8 training
│
├── best.pt                             # Best-performing PyTorch checkpoint
├── last.pt                             # Final-epoch PyTorch checkpoint
├── best.onnx                           # ONNX-exported model
├── best_ncnn_model/                    # NCNN-exported model (embedded/CPU inference)
│
├── results.csv                         # Training/validation metrics (tabular)
├── results.json                        # Training/validation metrics (structured)
├── plots/                              # Training curves, PR curves, confusion matrices
├── sample_detections/                  # Example inference outputs on test images
│
├── Report.pdf                          # Full write-up of methodology and results
├── .gitignore
└── LICENSE
```

---

##  Hardware Context

### Target Deployment Platform
- **Raspberry Pi–class embedded CPU** (or equivalent low-power edge device)
- No GPU / neural accelerator assumed — benchmark is deliberately CPU-only
- Intended for mounting on a small mobile robot for road-surface inspection

### Development Environment
- Trained using free-tier GPU compute (Kaggle)
- Benchmarked separately under CPU-only conditions to simulate real deployment constraints

---

##  Software Requirements

### Core Dependencies
```bash
Python 3.9+
ultralytics
torch
onnx
onnxruntime
ncnn
opencv-python
numpy
pandas
matplotlib
```

### Installation
```bash
pip install ultralytics onnx onnxruntime ncnn opencv-python numpy pandas matplotlib
```

---

## ️ Usage

### Inference — PyTorch
```python
from ultralytics import YOLO

model = YOLO("best.pt")
results = model.predict("path/to/image.jpg", save=True)
```

### Inference — ONNX
```python
from ultralytics import YOLO

model = YOLO("best.onnx")
results = model.predict("path/to/image.jpg", save=True)
```

### Inference — NCNN (edge-optimized)
```python
from ultralytics import YOLO

model = YOLO("best_ncnn_model")
results = model.predict("path/to/image.jpg", save=True)
```

### Training (from scratch or fine-tuning)
```bash
yolo detect train data=data.yaml model=yolov8n.pt epochs=400 imgsz=640
```

Full training configuration, augmentation settings, and export commands are documented step-by-step in `pothole-detection-for-robot.ipynb`.

---

##  Results

| Format   | Notes                                            |
|----------|---------------------------------------------------|
| PyTorch  | Baseline accuracy and latency reference           |
| ONNX     | Cross-platform export, CPU inference comparison   |
| NCNN     | Embedded/ARM-optimized, primary edge deployment candidate |

> Detailed latency (ms/frame), FPS, and accuracy (mAP/precision/recall) figures for each format are reported in `results.csv`, `results.json`, and discussed in full in `Report.pdf`.

Training curves, PR curves, and confusion matrices are available in `plots/`. Qualitative detection examples on held-out images are in `sample_detections/`.

---

##  Research Applications

### Current Use Case
- Feasibility study for deploying real-time object detection on CPU-only embedded hardware
- Baseline for integrating pothole detection into an autonomous or tele-operated inspection robot

### Future Research Directions
- Integration with a mobile robot platform for live road-surface scanning
- Quantization-aware training / INT8 export for further latency reduction
- Multi-class damage detection (cracks, patches, potholes) beyond single-class
- On-device deployment benchmarking on actual Raspberry Pi hardware (vs. simulated CPU-only benchmark)
- GPS-tagged pothole logging for municipal road maintenance datasets

---

##  Educational Value

### Learning Outcomes
- **Computer Vision:** Object detection, YOLO architecture, model fine-tuning
- **Model Deployment:** Cross-format export (PyTorch → ONNX → NCNN), embedded inference
- **Systems Engineering:** CPU-only benchmarking methodology, latency/accuracy trade-off analysis
- **Robotics:** Practical considerations for perception on resource-constrained platforms

---

##  License

This project is licensed under the **MIT License**.

See the [`LICENSE`](LICENSE) file for full terms.

---

##  Author

**Satwik Shreshth**
MCA Final Year, Sikkim University
Research Areas: Machine Learning, Computer Vision, Embedded/Edge Systems, Robotics

**GitHub:** [https://github.com/satwik-shreshth](https://github.com/satwik-shreshth)
**Repository:** [pothole-detection-edge-yolov8n](https://github.com/satwik-shreshth/pothole-detection-edge-yolov8n)

---

##  Intended Use

### Primary Applications
- Academic and personal research in edge AI / embedded computer vision
- Baseline system for robotics coursework or lab projects
- Reference benchmark for PyTorch → ONNX → NCNN deployment workflows

---

##  Known Limitations

- Benchmark is CPU-only and simulated; real on-device Raspberry Pi latency may differ
- Single-class detection (potholes only) — no severity classification or size estimation
- Dataset size and diversity may limit generalization to unseen road/lighting conditions
- No temporal tracking across video frames (frame-by-frame detection only)

---

##  Disclaimer

This software is provided *"as is"* for research and educational purposes.

- The author assumes no liability for misuse or reliance on detection outputs in safety-critical contexts
- Not certified for use in autonomous vehicle or public-safety systems
- Results should be independently validated before any operational use

---

##  Citation

If you use this work in academic research, please cite it as:

### BibTeX
```bibtex
@software{shreshth2026pothole,
  author  = {Shreshth, Satwik},
  title   = {Pothole Detection on the Edge: Fine-Tuned YOLOv8n with
             Cross-Format CPU Benchmarking for Embedded Deployment},
  year    = {2026},
  url     = {https://github.com/satwik-shreshth/pothole-detection-edge-yolov8n},
  note    = {Open-source edge deployment benchmark for YOLOv8n pothole detection}
}
```

### APA Style
```
Shreshth, S. (2026). Pothole Detection on the Edge: Fine-Tuned YOLOv8n with
Cross-Format CPU Benchmarking for Embedded Deployment [Computer software].
GitHub. https://github.com/satwik-shreshth/pothole-detection-edge-yolov8n
```

### IEEE Style
```
S. Shreshth, "Pothole Detection on the Edge: Fine-Tuned YOLOv8n with
Cross-Format CPU Benchmarking for Embedded Deployment," 2026. [Online].
Available: https://github.com/satwik-shreshth/pothole-detection-edge-yolov8n
```

---

##  Acknowledgments

- Ultralytics for the YOLOv8 framework
- ONNX and Tencent NCNN communities for cross-platform inference tooling
- Kaggle for free GPU compute used during training

---

##  Support & Contributions

### Reporting Issues
- Use GitHub Issues for bug reports
- Provide detailed steps to reproduce, including format (PyTorch/ONNX/NCNN) and hardware used

### Contributing
- Fork the repository
- Create a feature branch
- Submit a pull request with a clear description

---

**Status:** Active Development
**Model:** YOLOv8n
**Deployment Target:** CPU-only / Embedded (Raspberry Pi–class)

---

*Built for advancing real-time perception on resource-constrained robotics platforms.*