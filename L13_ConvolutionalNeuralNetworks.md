# Lesson 13: Convolutional Neural Networks

## Class Notes

### 1. Intro to CNNs

- **Statistical Invariance:** when knowing the inputs can contain the same kind of information, we share the weights; the images will usually contain things that don't change on average across time or space.
- **Intuition to CNNs:** when seeing a picture of Golden Retriever, human break-down the image into a combination of features, e.g. nose, eyes, fur color, etc. to make a decision. CNNs learns to do the same on its own.

### 2. Implementing CNNs

- **Breaking-up Images:** The first step for a CNN is to define a spatial filter (with width and height) to break-up the images into smaller pieces. The filter looks at small pieces, or patches, of the image. These patches are the same size as the filter.
- **Padding:**
  - Valid padding: never go outside of the edge of image
  - Same padding: use zeros to replace the area outside of the edge of image
- **Filter Depth:** We can use more than one filter, and filters can have different sizes. The amount of filters in a convolutional layer is called the *filter depth*.

- **Dimensionality:** Given:

  - our input layer has a width of `W` and a height of `H`
  - our convolutional layer has a filter size `F`
  - we have a stride of `S`
  - a padding of `P`
  - and the number of filters `K`,

  the following formula gives us the width of the next layer: `W_out =[ (W−F+2P)/S] + 1`.

  The output height would be `H_out = [(H-F+2P)/S] + 1`.

  And the output depth would be equal to the number of filters `D_out = K`.

  The output volume would be `W_out * H_out * D_out`.

  Knowing the dimensionality of each additional layer helps us understand how large our model is and how our decisions around filter size and stride affect the size of our network.

### 3. CNNs in TensorFlow:

- **TensorFlow Convolution Layer:** TensorFlow provides the [`tf.nn.conv2d()`](https://www.tensorflow.org/api_docs/python/tf/nn/conv2d) and [`tf.nn.bias_add()`](https://www.tensorflow.org/api_docs/python/tf/nn/bias_add) functions to create your own convolutional layers.

  ```python
  # Output depth
  k_output = 64

  # Image Properties
  image_width = 10
  image_height = 10
  color_channels = 3

  # Convolution filter
  filter_size_width = 5
  filter_size_height = 5

  # Input/Image
  input = tf.placeholder(
      tf.float32,
      shape=[None, image_height, image_width, color_channels])

  # Weight and bias
  weight = tf.Variable(tf.truncated_normal(
      [filter_size_height, filter_size_width, color_channels, k_output]))
  bias = tf.Variable(tf.zeros(k_output))

  # Apply Convolution
  conv_layer = tf.nn.conv2d(input, weight, strides=[1, 2, 2, 1], padding='SAME')
  # Add bias
  conv_layer = tf.nn.bias_add(conv_layer, bias)
  # Apply activation function
  conv_layer = tf.nn.relu(conv_layer)
  ```

  TensorFlow uses a stride for each `input` dimension, `[batch, input_height, input_width, input_channels]`. We are generally always going to set the stride for `batch` and `input_channels` (i.e. the first and fourth element in the `strides` array) to be `1`.

- **TensorFlow Max Pooling:** Pooling is a form of non-linear down-sampling, max pooling takes the maximum value of adjacent pixels. TensorFlow provides the `tf.nn.max_pool()` function to apply max pooling to your convolutional layers.

  ```python
  # Apply Max Pooling
  conv_layer = tf.nn.max_pool(
      conv_layer,
      ksize=[1, 2, 2, 1],
      strides=[1, 2, 2, 1],
      padding='SAME')
  ```

  The `ksize` and `strides` parameters are structured as 4-element lists, with each element corresponding to a dimension of the input tensor (`[batch, height, width, channels]`). For both `ksize` and `strides`, the batch and channel dimensions are typically set to `1`.

### 4. Visualizing Layers:

While neural networks can be a great learning device they are often referred to as a black box. However, we can visualize the feature maps and see what characteristics the network is utilizing.

```python
# image_input: the test image being fed into the network to produce the feature maps
# tf_activation: should be a tf variable name used during your training procedure that represents the calculated state of a specific weight layer
# Note: that to get access to tf_activation, the session should be interactive which can be achieved with the following commands.
# sess = tf.InteractiveSession()
# sess.as_default()

# activation_min/max: can be used to view the activation contrast in more detail, by default matplot sets min and max to the actual min and    max values of the output
# plt_num: used to plot out multiple different weight feature map sets on the same block, just extend the plt number for each new feature map entry

def outputFeatureMap(image_input, tf_activation, activation_min=-1, activation_max=-1 ,plt_num=1):
    # Here make sure to preprocess your image_input in a way your network expects
    # with size, normalization, ect if needed
    # image_input =
    # Note: x should be the same name as your network's tensorflow data placeholder variable
    # If you get an error tf_activation is not defined it maybe having trouble accessing the variable from inside a function
    activation = tf_activation.eval(session=sess,feed_dict={x : image_input})
    featuremaps = activation.shape[3]
    plt.figure(plt_num, figsize=(15,15))
    for featuremap in range(featuremaps):
        plt.subplot(6,8, featuremap+1) # sets the number of feature maps to show on each row and column
        plt.title('FeatureMap ' + str(featuremap)) # displays the feature map number
        if activation_min != -1 & activation_max != -1:
            plt.imshow(activation[0,:,:, featuremap], interpolation="nearest", vmin =activation_min, vmax=activation_max, cmap="gray")
        elif activation_max != -1:
            plt.imshow(activation[0,:,:, featuremap], interpolation="nearest", vmax=activation_max, cmap="gray")
        elif activation_min !=-1:
            plt.imshow(activation[0,:,:, featuremap], interpolation="nearest", vmin=activation_min, cmap="gray")
        else:
            plt.imshow(activation[0,:,:, featuremap], interpolation="nearest", cmap="gray")
```