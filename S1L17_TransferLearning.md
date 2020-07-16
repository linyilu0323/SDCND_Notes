# Lesson 17: Transfer Learning

## Class Notes

### 1. Intro

- Building from a pre-trained Neural Network or repurpose it for a similar task.
- GPU vs CPU: GPU is optimized for high throughput (because need to update all pixels on a game screen at the same time), and it happens to work well for deep learning too.

### 2. Use Cases:

Depending on both: (1) the size of the new dataset; AND (2) the similarity of the new data set to the original data set, the approach for using transfer learning will be different.

See detailed discussion.

### 3. Pre-Trained Networks

- **VGG:** is a pre-trained imagenet neural network, it has simple and elegant architecture, thus being frequently used as a starting point for working on other image classification tasks. To load VGG network in Keras, follow below example:

  ```python
  from keras.applications.vgg16 import VGG16
  model = VGG16(weights='imagenet', include_top=False)
  ```
  where, the first input is loading the pre-trained "imagenet" weights, and the argument `include_top` is for whether you want to include the fully-connected layer at the top of the network; unless you are actually trying to classify ImageNet's 1,000 classes, you likely want to set this to `False` and add your own additional layer for the output you desire.

Another aspect to pay attention is that the pre-trained network usually requires a specific type of pre-processing. Also, VGG requires input to be 224x224. Follow below example for pre-processing and re-sizing:

  ```python
  from keras.preprocessing import image
  from keras.applications.vgg16 import preprocess_input
  import numpy as np

  img_path = 'your_image.jpg'
  img = image.load_img(img_path, target_size=(224, 224))
  x = image.img_to_array(img)
  x = np.expand_dims(x, axis=0)
  x = preprocess_input(x)
  ```

- **GoogLeNet:** implements "Inception Layers" -  instead of choosing what kind of operation at each step: pooling, convolution; we take them all. Inception is also one of the models included in Keras Applications. Follow below examples:

  ```python
  from keras.applications.inception_v3 import InceptionV3
  model = InceptionV3(weights='imagenet', include_top=False)
  ```

  GoogLeNet takes 299x299 input.

- **ResNet:** further reduces the error of imagenet classification, it has 152 layers.

  ```python
  from keras.applications.resnet50 import ResNet50
  model = ResNet50(weights='imagenet', include_top=False)
  ```

  ResNet takes 299x299 input.
