# Udacity Self-Driving Car Nanodegree
# Lesson 6: Camera Calibration

## Class Notes

1. Challenges with Camera:
- Camera images transform 3D real-world information to a 2D image.
- Pinhole camera model: camera without lens, transform function maps 3D information to 2D.
- Actual camera model: real cameras use curved lenses to form image, this introduces **"radial distortion"**; another type of distortion, is when a camera's lens is not aligned perfectly parallel to the imaging plane, referred to as **"tangential distortion"**.

2. Correcting for Distortion
- Measuring Distortion: the first step is to "measure" the distortion and come up with a transform function to "undistort" the image. To do so, we can take pictures of known shape - a chessboard image is usually used to achieve this.
