# Lesson 12: Deep Neural Networks

## Class Notes

### 1. Intro to Deep Neural Networks

- Linear Models are limited.

- ReLU (Rectified Linear Units): a special type of activation function

  - Value is zero at x<0; and value is y=x at x>0. It can be represented as `f(x) = max(0, x)`.

  - TensorFlow provides `tf.nn.relu()`, it can be used as below:

    ```python
    # Hidden Layer with ReLU activation function
    hidden_layer = tf.add(tf.matmul(features, hidden_weights), hidden_biases)
    hidden_layer = tf.nn.relu(hidden_layer)
    
    output = tf.add(tf.matmul(hidden_layer, output_weights), output_biases)
    ```

- The Chain Rule: run forward prop and backward prop to get the derivatives .

### 2. DNN in TensorFlow:

- Resources: You can find this and many more examples of TensorFlow at [Aymeric Damien's GitHub repository](https://github.com/aymericdamien/TensorFlow-Examples).

- **Import Dataset:** use MNIST dataset provided by TensorFlow

- **Hidden Layer Parameters:** The variable `n_hidden_layer` determines the size of the hidden layer in the neural network. This is also known as the width of a layer.

- **Weights and Biases:** Deep neural networks use multiple layers with each layer requiring it's own weight and bias. The `'hidden_layer'` weight and bias is for the hidden layer. The `'out'` weight and bias is for the output layer. If the neural network were deeper, there would be weights and biases for each additional layer.
  
- **Input:** The MNIST data is made up of 28px by 28px images. The [`tf.reshape()`](https://www.tensorflow.org/versions/master/api_docs/python/tf/reshape) function above reshapes the 28px by 28px matrices in `x` into row vectors of 784px.

- **Multilayer Perceptron:** Add a ReLU in between hidden layer output and the output layer input.

- **Optmizier:** Define the cost function first, then use optimizer (Gradient Descent) to get to the minimum.

- **Session:** In the session, break the full dataset into small batches, and train each batch.

  ```python
  from tensorflow.examples.tutorials.mnist import input_data
  mnist = input_data.read_data_sets(".", one_hot=True, reshape=False)
  
  import tensorflow as tf
  
  # Parameters
  learning_rate = 0.001
  training_epochs = 20
  batch_size = 128  # Decrease batch size if you don't have enough memory
  display_step = 1
  
  n_input = 784  # MNIST data input (img shape: 28*28)
  n_classes = 10  # MNIST total classes (0-9 digits)
  
  n_hidden_layer = 256 # layer number of features
  
  # Store layers weight & bias
  weights = {
      'hidden_layer': tf.Variable(tf.random_normal([n_input, n_hidden_layer])),
      'out': tf.Variable(tf.random_normal([n_hidden_layer, n_classes]))
  }
  biases = {
      'hidden_layer': tf.Variable(tf.random_normal([n_hidden_layer])),
      'out': tf.Variable(tf.random_normal([n_classes]))
  }
  
  # tf Graph input
  x = tf.placeholder("float", [None, 28, 28, 1])
  y = tf.placeholder("float", [None, n_classes])
  
  x_flat = tf.reshape(x, [-1, n_input])
  
  # Hidden layer with RELU activation
  layer_1 = tf.add(tf.matmul(x_flat, weights['hidden_layer']), biases['hidden_layer'])
  layer_1 = tf.nn.relu(layer_1)
  # Output layer with linear activation
  logits = tf.matmul(layer_1, weights['out']) + biases['out']
  
  # Define loss and optimizer
  cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=logits, labels=y))
  optimizer = tf.train.GradientDescentOptimizer(learning_rate=learning_rate).minimize(cost)
  
  # Initializing the variables
  init = tf.global_variables_initializer()
  
  # Launch the graph
  with tf.Session() as sess:
      sess.run(init)
      # Training cycle
      for epoch in range(training_epochs):
          total_batch = int(mnist.train.num_examples/batch_size)
          # Loop over all batches
          for i in range(total_batch):
              batch_x, batch_y = mnist.train.next_batch(batch_size)
              # Run optimization op (backprop) and cost op (to get loss value)
              sess.run(optimizer, feed_dict={x: batch_x, y: batch_y})
          # Display logs per epoch step
          if epoch % display_step == 0:
              c = sess.run(cost, feed_dict={x: batch_x, y: batch_y})
              print("Epoch:", '%04d' % (epoch+1), "cost=", \
                  "{:.9f}".format(c))
      print("Optimization Finished!")
  
      # Test model
      correct_prediction = tf.equal(tf.argmax(logits, 1), tf.argmax(y, 1))
      # Calculate accuracy
      accuracy = tf.reduce_mean(tf.cast(correct_prediction, "float"))
      # Decrease test_size if you don't have enough memory
      test_size = 256
      print("Accuracy:", accuracy.eval({x: mnist.test.images[:test_size], y: mnist.test.labels[:test_size]}))
  ```

### 3. Save and Restore Models

- **Save TF Models:** TensorFlow gives you the ability to save your progress using a class called [`tf.train.Saver`](https://www.tensorflow.org/api_docs/python/tf/train/Saver). This class provides the functionality to save any [`tf.Variable`](https://www.tensorflow.org/api_docs/python/tf/Variable) to your file system. For example:

  ```python
  import tensorflow as tf
  
  # The file path to save the data
  save_file = './model.ckpt'
  
  # Two Tensor Variables: weights and bias
  weights = tf.Variable(tf.truncated_normal([2, 3]))
  bias = tf.Variable(tf.truncated_normal([3]))
  
  # Class used to save and/or restore Tensor Variables
  saver = tf.train.Saver()
  
  with tf.Session() as sess:
      # Initialize all the Variables
      sess.run(tf.global_variables_initializer())
  
      # Save the model
      saver.save(sess, save_file)
  ```
  
- **Restore TF Models:** To load saved models, use below example:

  ```python
  # Remove the previous weights and bias
  tf.reset_default_graph()

  # Two Variables: weights and bias - you will need to create the tensors first, then use "restore" function to load data
  weights = tf.Variable(tf.truncated_normal([2, 3]))
  bias = tf.Variable(tf.truncated_normal([3]))

  # Class used to save and/or restore Tensor Variables
  saver = tf.train.Saver()

  with tf.Session() as sess:
      # Load the weights and bias
      saver.restore(sess, save_file)
  ```

- **"Assign requires shapes of both tensors to match" Error**: TensorFlow will assign name automatically if one is not given, this might result in a mismatch between the saved parameter and the parameter to restore, causing a "mismatch" error. So, instead of letting TensorFlow set the `name` property, let's set it manually:

  ```python
  #=============== NOT-AS-GOOD WAY ===============
  # Two Tensor Variables: weights and bias
  weights = tf.Variable(tf.truncated_normal([2, 3]))
  bias = tf.Variable(tf.truncated_normal([3]))
  
  #=============== BETTER WAY ===============
  # Two Tensor Variables: weights and bias
  weights = tf.Variable(tf.truncated_normal([2, 3]), name='weights_0')
  bias = tf.Variable(tf.truncated_normal([3]), name='bias_0')
  
  #Also define the same name when creating variables for loading.
  ```

### 4. Avoid over-fitting:

  - **Theories:** we usually start with a large network structure, if not careful, it could end up in over-fitting, losing the capability to make proper predictions. A few ways to avoid over-fitting:
      - Early-termination: look at validation set performance vs. training time, as soon as stopping improving, stop train.
      - Regularization: apply artificial constraints to the network, reduce the number of free parameters.
          - L2 Regularization: add another term to the loss function which penalizes large weights, usually it's represented by a small parameter, multiplied by the L2 norm of the weight vector.
      - Dropout: randomly reset half of the data flowing from one layer to the next (activation) to zero - the idea is that the model should never over-relying on any one single component of the activation.

- **TensorFlow Dropout:** TensorFlow provides the [`tf.nn.dropout()`](https://www.tensorflow.org/api_docs/python/tf/nn/dropout) function, which you can use to implement dropout. Example:

  ```python
  keep_prob = tf.placeholder(tf.float32) # probability to keep units
  
  hidden_layer = tf.add(tf.matmul(features, weights[0]), biases[0])
  hidden_layer = tf.nn.relu(hidden_layer) 
  # above are standard steps to calculate with weights/biases and perform ReLU.
  hidden_layer = tf.nn.dropout(hidden_layer, keep_prob)
  # dropout takes the "hidden_layer" result from standard process, then takes the probability of keeping given unit.
  
  logits = tf.add(tf.matmul(hidden_layer, weights[1]), biases[1])
  
  with tf.Session() as sess:
      sess.run(tf.global_variables_initializer())
      output = sess.run(logits, feed_dict={keep_prob: 0.5})
      print(output)
  ```

  During training, a good starting value for `keep_prob` is `0.5`.

  During testing, use a `keep_prob` value of `1.0` to keep all units and maximize the power of the model.

