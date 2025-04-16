README.txt
----------

### Project Directory Overview

This project contains two main problem statements, each with its own complete workflow for training, testing, and deployment:

---

1. Ground Segmentation

Folder Name: Ground_Segmentation

- Subfolders:
  - training_script/ – Contains the training code for ground segmentation.
  - testing_script/ – Includes scripts for testing the trained model.
  - convert_to_ONNX/ – Contains code to export the trained model to ONNX format.

  > Please refer to Ground_Segmentation_README-2 for a complete explanation and usage guide.

---

2. Pallet Detector

Folder Name: Pallet_Detector

- Subfolders:
  - training_script/ – Contains the training code for pallet detection.
  - testing_script/ – Includes scripts for evaluating the trained model.
  - convert_to_ONNX/ – Contains code to convert the trained model into ONNX format.

  > Please refer to Pallet_Detection_README-2 for full details and workflow instructions.

---

3. Model Links

File: model_links.txt  
Contains direct links to the trained model files for each task (segmentation and detection), organized by category and purpose.

---

4. ROS2 Implementation

- Dockerized ROS2 node - link in the 'ROS2_README.txt' .
- ROS2_README.txt: Detailed guide on how the Dockerized ROS2 nodes for ground segmentation and pallet detection have been implemented.
- ros2_ws.zip: A compressed version of the complete ROS2 workspace (without Docker).
- ros2_video_link.txt: Contains the link to a video demonstrating the ROS2 implementation in action.

  > For the Dockerized version of the ROS2 workspace, please refer to the Docker section in ROS2_README.txt for build and run instructions.

---

Thank you for taking the time to go through this package.  
