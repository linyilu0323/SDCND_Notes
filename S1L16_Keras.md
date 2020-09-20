# Lesson 16: Keras

## Class Notes

### 1. Keras Intro and Overview

- Keras is a deep learning framework built based on TensorFlow. [Keras](https://faroit.github.io/keras-docs/1.2.1/) makes coding deep neural networks simpler.

### 2. Building Neural Networks in Keras

- **Neural Networks:** A keras model can be created by the [keras.models.Sequential](https://keras.io/models/sequential/) class, it provides common functions like `fit()`, `evaluate()`, and `compile()`.

  - `fit()` is used to run the training:

    `history = model.fit(X_normalized, y_one_hot, epochs=10, validation_split=0.2)`

  - `evaluate()` is used to evaluate model with new set of data (usually the test set):

    `metrics = model.evaluate(X_normalized_test, y_one_hot_test)`

  - `compile()` is used to build the model and optimizer.

    `model.compile('adam', 'categorical_crossentropy', ['accuracy'])`

- **Layers:** A Keras layer is just like a neural network layer. There are fully connected layers, max pool layers, and activation layers. You can add a layer to the model using the model's `add()` function. For example, a simple model would look like this:

  ```python
  from keras.models import Sequential
  from keras.layers.core import Dense, Activation, Flatten
  model = Sequential()
  model.add(Flatten(input_shape=(32, 32, 3)))
  model.add(Dense(100))
  model.add(Activation('relu'))
  model.add(Dense(60))
  model.add(Activation('relu'))
  model.add(Activation('softmax'))
  ```

  Keras will automatically infer the shape of all layers after the first layer. This means you only have to set the input dimensions for the first layer.

  **Example:** https://github.com/keras-team/keras/blob/master/examples/mnist_mlp.py

- **Convolution:** Use similar "add layer" method, but this time, with `Conv2D` functions:

  ```python
  from keras.layers.convolutional import Conv2D

  model.add(Conv2D(32, kernel_size=(3, 3),
                   activation='relu',
                   input_shape=input_shape))
  ```

  **Example: ** https://github.com/keras-team/keras/blob/master/examples/mnist_cnn.py

- **Pooling:** with `MaxPooling2D` functions:

  ```python
  from keras.layers.pooling import MaxPooling2D

  model.add(MaxPooling2D(pool_size=(2,2)))
  ```

- **Dropout:** with `Dropout` functions:

	```python
	from keras.layers import Dropout

	model.add(Dropout(0.5))
	```

- **Testing:** Use `model.evaluate` method to see how good/bad the model does.
