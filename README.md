# README
*My notes to Udacity's Self-Driving Car Nanodegree online courses.*

---

## ===== Semester 1 =====

### Section 1: Computer Vision

- Lesson 4: Computer Vision Fundamentals [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L4_ComputerVisionFundamentals.md)
- **Lesson 5 (Project)**: Finding Lane Lines [@Github](https://github.com/linyilu0323/CarND_P1_LaneLines)
  - In this project, the goal is to use the basic computer vision knowledge to develop an algorithm to detect lane lines. Main techniques used in the project are: Canny detection, Hough transform.
- Lesson 6: Camera Calibration [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L6_CameraCalibration.md)
- Lesson 7: Gradients and Color Spaces [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L7_GradientsAndColorSpaces.md)
- Lesson 8: Advanced Computer Vision
- **Lesson 9 (Project)**: Advanced Lane Finding [@Github](https://github.com/linyilu0323/CarND_P2_AdvancedLaneLines)
  - This project is an advanced version of Lesson 5, it employs more advanced techniques such as: camera calibration, distortion correction, perspective transform, combined thresholding (color, gradient, in different colorspace, etc.), warping the images.

---

### Section 2: Deep Learning

- Lesson 10: Neural Networks [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L10_NeuralNetworks.md)
- Lesson 11: TensorFlow [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L11_TensorFlow.md)
- Lesson 12: Deep Neural Networks [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L12_DeepNeuralNetworks.md)
- Lesson 13: Convolutional Neural Networks [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L13_ConvolutionalNeuralNetworks.md)
- Lesson 14: LeNet for Traffic Signs
- **Lesson 15 (Project)**: Traffic Sign Classifier [@Github](https://github.com/linyilu0323/CarND_P3_TrafficSignClass)
  - This project uses the basic Convolutional Neural Network knowledge to build a classifier for German traffic sign. A LeNet-like architecture is built from scratch by adding TensorFlow layers one-by-one.
- Lesson 16: Keras [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L16_Keras.md)
- Lesson 17: Transfer Learning [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L17_TransferLearning.md)
- **Lesson 18 (Project)**: Behavioral Cloning [@Github](https://github.com/linyilu0323/CarND_P4_BehaviorCloning)
  - This project uses Keras sequential models and trained a neural network to "clone" driving behavior recorded from training sessions, the training inputs are images from cameras mounted on the car (simulator generated images), the output is the steering angle at each frame. Once the model is trained, we use the trained model to drive in autonomous mode.

---

### Section 3: Sensor Fusion

- Lesson 20: Sensors
- Lesson 21: Kalman Filters
- Lesson 22: C++ Checkpoint
- Lesson 23: Geometry and Trigonometry Refresher
- Lesson 24: Extended Kalman Filters [Link to notes](https://github.com/linyilu0323/SDCND_Notes/blob/master/S1L24_KalmanFilter.md)
- **Lesson 25 (Project)**: Extended Kalman Filters Project [@Github](https://github.com/linyilu0323/CarND_P5_ExtendedKalmanFilter)
  - In this project, you will utilize a kalman filter to estimate the state of a moving object of interest with noisy lidar and radar measurements.
  
    


## ===== Semester 2 =====

### Section 4: Localization

- Lesson 1: Introduction to Localization
- Lesson 2: Markov Localization [Link to notes](S1L24_KalmanFilter.md)
- Lesson 3: Motion Models
- Lesson 4: Particle Filters
- Lesson 5: Implementation of Particle Filters [Link to notes](S1L24_KalmanFilter.md)
- **Lesson 6 (Project):** Kidnapped Vehicle Project [@Github](https://github.com/linyilu0323/CarND_P6_Localization)
  - In this project you will implement a 2 dimensional particle filter in C++, and try to find the exact location of the kidnapped vehicle with sensor data.

---

### Section 5: Planning

- Lesson 7: Search
- Lesson 8: Prediction [Link to notes](S2L7_PathPlanning.md)
- Lesson 9: Behavior Planning  [Link to notes](S2L9_BehaviorPlanning.md)
- Lesson 10: Trajectory Generation [Link to notes](S2L10_TrajectoryGeneration.md)
- **Lesson 11 (Project):** Highway Driving [@Github](https://github.com/linyilu0323/CarND_P7_PathPlanning)
  - In this project, your goal is to design a path planner that is able to create smooth, safe paths for the car to follow along a 3 lane highway with traffic.

---

### Section 6: Controls

- Lesson 12: PID Control [Link to notes](S2L12_Controls.md)
- **Lesson 13 (Project):** PID Controller [@Github](https://github.com/linyilu0323/CarND_P8_PID_Control)
  - In this project you will fine tune a PID controller to keep the car within the lanes and drive as fast as possible.

---

### Section 7: ROS - Robot Operating System

- Lesson 14: Autonomous Vehicle Architecture [Link to notes](S2L14_SystemIntegration.md)
- Lesson 15: Introduction to ROS
- Lesson 16: Writing ROS Nodes
- Lesson 17: Packages & Catkin Workspaces [Link to notes](S2L15_ROS.md)


## Capstone Project
- **Project: System Integration** [@Github](https://github.com/linyilu0323/CarND_Capstone)