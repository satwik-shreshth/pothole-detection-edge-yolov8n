# Pothole Detection on the Edge — YOLOv8n

Fine-tuned YOLOv8n for real-time pothole detection, with a CPU-only benchmark comparing **PyTorch**, **ONNX**, and **NCNN** export formats to evaluate feasibility for embedded/edge deployment (e.g. Raspberry Pi–class robots).

## Overview

This project trains a YOLOv8n object detector to identify potholes in road imagery, then benchmarks three deployment-ready export formats on CPU to determine the best trade-off between inference latency and accuracy for resource-constrained edge devices. It was built with an eye toward deployment on a mobile robot platform.

## Repository Structure

```
.
├── pothole-detection-for-robot.ipynb   # End-to-end training, export, and benchmarking notebook
├── data.yaml                           # Dataset config (classes, paths) for YOLOv8 training
├── best.pt / last.pt                   # Best and final PyTorch checkpoints
├── best.onnx                           # ONNX-exported model
├── best_ncnn_model/                    # NCNN-exported model (for CPU/embedded inference)
├── results.csv / results.json          # Training and evaluation metrics
├── plots/                              # Training curves, PR curves, confusion matrices, etc.
├── sample_detections/                  # Example inference outputs on test images
└── Report.pdf                          # Full write-up of methodology and results
```

## Model

- **Architecture:** YOLOv8n (Ultralytics), fine-tuned for single-class pothole detection
- **Export formats benchmarked:**
  - PyTorch (`.pt`) — baseline
  - ONNX (`.onnx`) — cross-platform inference
  - NCNN — optimized for ARM/embedded CPU inference

## Getting Started

### Requirements

```bash
pip install ultralytics onnx onnxruntime ncnn
```

### Inference

```python
from ultralytics import YOLO

# PyTorch
model = YOLO("best.pt")

# ONNX
model = YOLO("best.onnx")

# NCNN
model = YOLO("best_ncnn_model")

results = model.predict("path/to/image.jpg", save=True)
```

### Training

Training details, hyperparameters, and the full experimental pipeline are documented in `pothole-detection-for-robot.ipynb` and `Report.pdf`.

## Results

See `results.csv`, `results.json`, and `plots/` for detailed training and validation metrics, and `sample_detections/` for qualitative examples. Full discussion of the CPU benchmark comparison across export formats is in `Report.pdf`.

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
