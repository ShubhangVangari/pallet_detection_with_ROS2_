# Visualizing Sea Map - ROS2 Inference Project

This repository contains a complete ROS2 project for **Pallet Detection** and **Ground Segmentation** using ONNX models. The system includes ROS2 nodes for image publishing and inference, along with a Dockerized setup for deployment on any compatible system (e.g., Ubuntu, Jetson, GCP VM).


### DRIVE LINK : the Dockerised files were to big to upload onto Github, please follow the link.
link : https://drive.google.com/drive/folders/1kPiwHhaFPrjIXosKpRpQBHQ7XC_Z9Hwi?usp=sharing
---

## 📁 Project Structure

```
ros2_docker_test/
├── Dockerfile
├── entrypoint.sh
├── ros2_wc/
│   ├── models/                 # Contains ONNX models
│   ├── test_images/           # Input images for testing
│   ├── results/               # Output images with visualized inference
│   └── src/
│       └── pallet_seg_node/
│           ├── pallet_seg_node/
  │           ├── image_publisher.py
  │           ├── subscriber.py
  │           ├── detector_utils.py
  │           ├── segment_utils.py
  │           ├── __init__.py
  │           ├── setup.py
  │           └── package.xml

---

## 🔧 ROS2 Package Details

### Nodes Implemented

1. **image_publisher**  
   Publishes images from a folder to the `/camera/image_raw` topic.

2. **inference_subscriber**  
   Subscribes to `/camera/image_raw`, runs ONNX inference for:
   - **Pallet Detection (YOLOv8 ONNX)**
   - **Ground Segmentation (DeepLabV3+ ONNX)**  
   Overlays results and saves output to the `results/` folder.


### Number of publishers and subscribers

1. There is one main image_publisher - that publishes the images for the subscriber node to infer.
2. Once the subscriber node infers, it also re-publishes over three topic
  2.1. the first topic only publishes the segmentation mask
  2.2. the second topic only publshes the pallets detection 
  2.3. the third and last topic publishes both the segmentation mask and bounding boxes together


### Topic Info

- First Topic : `/camera/image_raw`
- Second Topic : '/gorund_segmentation'
- Third_Topic : '/Plattet_detection_raw'
- Fourth_Topic : '/Pallet_detection_masked'

---

## 🧠 Model Details

- **Pallet Detection**: YOLOv8 trained on Roboflow dataset (~3000 images).  
  Fine-tuned using Mixed Precision on an augmented dataset of ~5000 images.
  
- **Ground Segmentation**: DeepLabV3+ with MobileNet backbone. Trained on over 1000 images with multiple weather augmentations.

- **Export Format**:  
  Both models are exported to **ONNX** for optimized inference.

> **Why ONNX?**  
> - Lightweight and hardware-agnostic  
> - Faster inference with optimized runtimes (ONNX Runtime, TensorRT)  
> - Easily deployable across edge devices like Jetson and cloud VMs

---

## 🐳 Dockerized Inference

### Build the Docker Image (if starting from code)

```bash
cd ros2_docker_test
sudo docker build -t ros2_test .
```

---

## 🔁 Full Docker Workflow (After Download or Build)

### 🚀 Run the Docker Container

```bash
sudo docker run -it --name ros2_test_node ros2_test
```

---

### ▶️ Launch the Publisher Node in the Container

```bash
sudo docker exec -it ros2_test_node bash
source /opt/ros/humble/setup.bash
source /ros2_ws/install/setup.bash
ros2 run pallet_seg_node image_publisher
```

---

### ▶️ Launch the Inference Subscriber Node in the Same Container

```bash
sudo docker exec -it ros2_test_node bash
source /opt/ros/humble/setup.bash
source /ros2_ws/install/setup.bash
ros2 run pallet_seg_node inference_subscriber
```

---

### 📂 View the Results Inside Docker

```bash
sudo docker exec -it ros2_test_node bash
cd /ros2_ws/results
ls -l
```

---

### 📥 Download the Results to Local Machine

```bash
sudo docker cp ros2_test_node:/ros2_ws/results ./results_from_docker
```

---

## ✅ Deliverables Summary

- ✅ ROS2 Node for Inference (Subscriber + Publisher)  
- ✅ ONNX Model Integration  
- ✅ Dockerized ROS2 Workspace  
- ✅ Test Images and Result Generation  
- ✅ README + Requirements.txt + Source Code  

---

## 📎 Notes

- If you're using this on Jetson or GPU, consider replacing ONNX Runtime with TensorRT for maximum efficiency.
- The `test_images/` folder can be replaced with any set of images for batch inference.

---
