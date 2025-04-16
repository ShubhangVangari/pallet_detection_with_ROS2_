
# 📦 Pallet Detection – YOLOv8 Based Pipeline

---

## 🧩 Problem Statement

**Develop a robust object detection pipeline to accurately identify GMA and Euro pallets in diverse warehouse environments.**

The model should:
- Detect pallets across varying lighting and weather conditions
- Be evaluated using mAP
- Be trained and fine-tuned for robustness using augmentation and mixed precision

---

## 📁 Dataset Preparation

- **Original Dataset**: Downloaded from **Roboflow**
- **Initial Size**: ~3000 images (pallets in warehouse scenes)
- **Class Labels**: GMA Pallet, Euro Pallet
- **Post-Training Augmentation**:
  - Retained **40%** of original dataset (~1200 images)
  - Applied **heavy augmentation** on this set:
    - Brightness Adjusted
    - Snow
    - Rain
    - Fog
    - Jitters
- **Final Dataset for Fine-Tuning**: **5000+ images**

---

## 🚀 Initial Training – YOLOv8 Nano

- **Framework**: Ultralytics YOLOv8
- **Model**: `YOLOv8n` (`yolov8n.yaml`) – lightweight and fast
- **Training Script**: `model.train()` using the Ultralytics interface
- **Epochs**: 200
- **Image Size**: 640×640
- **Batch Size**: 32
- **Optimizer**: SGD
- **Learning Rate Schedule**: Cosine decay
  - `lr0 = 0.01`
  - `lrf = 0.001`
  - `momentum = 0.937`
  - `weight_decay = 0.0005`
- **Warmup Strategy**:
  - `warmup_epochs = 3`
  - `warmup_momentum = 0.8`
  - `warmup_bias_lr = 0.1`
- **Augmentation**: Enabled


---

## ⚙️ Fine-Tuning with Mixed Precision (AMP)

- **Base Model**: `TT_PD2_base_model_200_.pt`
- **Framework**: Ultralytics YOLOv8
- **Epochs**: 100
- **AMP**: Enabled using `amp=True` in training config
- **Optimizer**: SGD
  - `lr0 = 0.001`
  - `lrf = 0.01`
- **Augmentation**: Enabled (same 5-class variations as segmentation)
- **Warmup**: 3 epochs
- **Learning Rate Schedule**: Cosine decay
- **No Layer Freezing**: Entire model fine-tuned


---


---

### 🔄 ONNX Model Export

After training, the final model was also **exported to ONNX format** for optimized deployment.

#### ✅ Why ONNX?
- **Interoperability**: Can run on different platforms (PyTorch, TensorRT, OpenVINO, etc.)
- **Deployment Ready**: Ideal for edge devices, cloud APIs, and cross-framework use
- **Speed & Efficiency**: Enables hardware-specific optimizations like quantization and fusion
- **Lightweight**: Smaller model size and faster inference, especially useful for real-time robotics and embedded systems

