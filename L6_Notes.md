# Udacity Self-Driving Car Nanodegree
# Lesson 6: Camera Calibration

## Class Notes

1. Challenges with Camera:
- Camera images transform 3D real-world information to a 2D image.
- Pinhole camera model: camera without lens, transform function maps 3D information to 2D.
- Actual camera model: real cameras use curved lenses to form image, this introduces **"radial distortion"**; another type of distortion, is when a camera's lens is not aligned perfectly parallel to the imaging plane, referred to as **"tangential distortion"**.

2. Correcting for Distortion:
- **Measuring Distortion:** the first step is to "measure" the distortion and come up with a transform function to "undistort" the image. To do so, we can take pictures of known shape - a chessboard image is usually used to achieve this.  
- **Camera Calibration Function in OpenCV:**  
```python
ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)
```
objpoints - Vector of vectors of the "world" coordinates of the internal corners of the chessboard.
imgpoints -  Vector of vectors of the actual 2D coordinates corresponding to the internal corners found on any image. These coordinates can be retrieved by using:
'''python
ret, corners = cv2.findChessboardCorners(gray, img_size, None)
'''  
The '''calibrateCamera''' function returns below results:
mtx - camera matrix (also called intrinsic camera matrix, this represents )
dist - distortion coefficients (this describes the lens distortion)
rvecs, tvecs - representing the camera location
- **Correct Any Image:** With the calibrated parameters of a camera, we can now "undistort" any image taken with the same camera:
'''python
undistort = cv2.undistort(img, mtx, dist, None, mtx)
'''

3. Perspective Transform:
If we already know two sets of points: source and destination - source is the coordinates of four corner points of any quadrilateral on the original image; destination is the coordinates of four corner points of a rectangle on an image space you want to transform to - we can easily calculate the transform matrix by using:  
'''python
M = cv2.getPerspectiveTransform(src, dst)
'''
Then, the perspective transform can be done to the original image by:
'''python
warped = cv2.warpPerspective(img, M, img_size, flags=cv2.INTER_LINEAR)
'''
