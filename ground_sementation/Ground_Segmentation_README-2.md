
# 🛰️ Ground Segmentation - Project Overview


## 🧩 Problem Statement

**Develop a robust computer vision pipeline for ground segmentation in dynamic warehouse environments.**

The system should:

Segment the ground/floor area using semantic segmentation
Be trained and evaluated on a diverse dataset enriched with realistic environmental variations (lighting, snow, fog, etc.)
Achieve high performance segmentation (IoU) under varied conditions

---

### 📂 Dataset Preparation
- Initial dataset: **519** real-world warehouse images  
- **Filtered** out irrelevant samples manually  
- **Added 500 more images** from online warehouse datasets  
- Final dataset size: **~1000 images**
- Composition:
  - **40%** clean, unaltered images
  - **60%** heavily augmented to simulate real-world conditions:
    - **Brightness Adjusted** (sunlight glare)
    - **Snow**
    - **Rain**
    - **Fog**
    - **Jitters** (camera motion blur)

---

### 🟥 Segmentation Mask Generation (SAM)

- **Model**: Segment Anything Model (SAM) with **ViT-H** backbone  
- **Library**: `segment-anything` from Meta AI  
- **Weights**: `sam_vit_h_4b8939.pth`  

#### 🔁 Ground Mask Extraction Logic
1. **Read** the image
2. **Run inference** using `SamAutomaticMaskGenerator`
3. **Sort** all masks by area (descending)
4. **Pick** the largest mask
5. **Validate** mask's position:
   - Compute centroid Y-coordinate  
   - If **centroid_y > 60% of image height**, mark as **ground/floor mask**
   - Automates ground mask filtering, no manual intervention needed
6. **Convert** mask to binary (boolean → uint8)
7. **Create** red `(255, 0, 0)` mask overlay using `np.zeros`
8. **Merge** the red mask with the original RGB image
9. **Save**:
   - Binary mask
   - Mask-overlay image
   - Fully annotated image with **all SAM detections**
10. **Visualize** using `supervision.MaskAnnotator` and `sv.plot_images_grid`

---

### 🧠 Ground Segmentation Model Training

- **Model**: `DeepLabV3 + MobileNetV3-Large` from `torchvision`
- **Pretrained**: Yes (`pretrained=True`)
- **Input Resolution**: Resized to `512 × 512`
- **Loss Function**: `BCEWithLogitsLoss` (binary segmentation)
- **Optimizer**: `Adam`, learning rate = `1e-4`
- **Batch Size**:
  - Train: 4
  - Validation: 1
- **Epochs**: 70
- **Evaluation**:
  - Validation **IoU** and **Loss** tracked per epoch
  - Best model checkpoints saved with loss and IoU in filename
- **DataLoader**: Custom PyTorch `Dataset` class to load image-mask pairs

---

### ⚡ Mixed Precision Fine-Tuning

- **Objective**: Fine-tune the DeepLabV3 + MobileNetV3-Large model using **mixed precision training** to reduce memory usage and accelerate training
- **Framework**: PyTorch with `torch.cuda.amp` for mixed precision support
- **Model**: `DeepLabV3 + MobileNetV3-Large` (pretrained)
- **Loss Function**: `BCEWithLogitsLoss` (for binary segmentation tasks)
- **Optimizer**: `Adam`
- **Scheduler**: Learning rate adjusted based on validation loss
- **Dataset**: Custom `SegmentationDataset` loading from:
  - `/split_dataset/train/images`
  - `/split_dataset/train/masks`
- **Image Size**: All samples resized to **512 × 512**
- **Batch-level Mixed Precision**:
  - Forward pass wrapped with: `with autocast("cuda")`
  - Backward pass uses: `GradScaler().scale(loss).backward()`
  - Optimizer step and scaler update: `scaler.step()` and `scaler.update()`
- **Checkpoint Initialization**:
  - Fine-tuning starts from a previously trained model 
- **Benefit**:
  - Significantly faster training with lower GPU memory usage,  
    while retaining high segmentation accuracy

---

---

### 🔄 ONNX Model Export

After training, the final model was also **exported to ONNX format** for optimized deployment.

#### ✅ Why ONNX?
- **Interoperability**: Can run on different platforms (PyTorch, TensorRT, OpenVINO, etc.)
- **Deployment Ready**: Ideal for edge devices, cloud APIs, and cross-framework use
- **Speed & Efficiency**: Enables hardware-specific optimizations like quantization and fusion
- **Lightweight**: Smaller model size and faster inference, especially useful for real-time robotics and embedded systems

