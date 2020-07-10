# Lesson 7: Gradients and Color Spaces

## Class Notes

### 1. Gradient and Sobel Operator:

- We know that most car lanes appear to be "almost vertical" on camera images - this is something we can utilize to distinguish car lanes with others.
- Sobel Operator: this is the key part of the Canny edge detection algorithm, the mathematics is simply taking the derivative of the image in different directions:
```python
sobel_y = cv2.Sobel(gray, cv2.CV_64F, 0, 1)
```
- With the information of image gradient on x-direction (car lanes are usually along y-direction, meaning the x-direciton gradient is high), we can apply thresholding and filter out more related information:
```python
# apply sobel operator to image on x-direction to get sobelx
abs_sobelx = np.absolute(sobelx)
scaled_sobel = np.uint8(255*abs_sobelx/np.max(abs_sobelx))
sxbinary = np.zeros_like(scaled_sobel)
sxbinary[(scaled_sobel >= thresh_min) & (scaled_sobel <= thresh_max)] = 1
```

### 2. Magnitude and Direction of Gradient

- **The magnitude of gradient:**  
You can combine the sobel gradients on x and y directions to get the magnitude of gradient.  
- **The direction of gradient:**  
Given the magnitude of gradients on x and y axes, the direction can be calculated by using arctangent of sobel_y divided by sobel_x.
Note: here the result will be represented in radians, with zero value meaning horizontal line.
- **Combining the thresholds:**  
Usually the car lanes appear near vertically on camera images, and it shows larger gradients. We can combine these two features to better separate car lanes from the background.
```python
mag_sobel = np.sqrt(sobel_x**2 + sobel_y**2)
dir_sobel = np.arctan2(sobel_y, sobel_x)
```

### 3. Color Spaces

- **Notes working with RGB color space:**  
If you read in an image using ```matplotlib.image.imread()``` you will get an RGB image, but if you read it in using OpenCV ```cv2.imread()``` this will give you a BGR image.  
This is critical as when you need to convert RGB (or BGR) images to another color space of your choice using ```cv2.cvtColor``` command, you will want to make sure you specified the correct input.  

- **Alternatives to RGB color space:**  
In addition to RGB color space, which builds on 3 basic colors (red, green, and blue), it is sometimes more preferable to use other color spaces for easier processing and better separation. They include:  
*HLS Color Space*: HLS = Hue, Lightness, and Saturation.
*HSV Color Space*: HSV = Hue, Saturation, and Value.
Read more about [HLS and HSV](https://en.wikipedia.org/wiki/HSL_and_HSV).

4. Combine Color and Gradient Thresholding

