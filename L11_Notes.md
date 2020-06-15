# Lesson 11: Tensor Flow

## Class Notes

### 1. Deep Learning

- Deep learning is a branch of machine learning, it utilizes large amount of data, and let the computer to "learn" itself.

## 2. Tensor Flow:

- **Install TensorFlow:**

  - Solution to problems with "module 'tensorflow' has no attribute 'session'" due to TF2.x.

    ```python
    import tensorflow.compat.v1 as tf
    tf.disable_v2_behavior()
    ```

    *OR*

    `tf.compat.v1.Session()`

- **Tensor:** is the basic object where data is being stored at, there are lots of types of tensor, for example:

  - `tf.constant()`: is a constant tensor, the value never changes.

  - `tf.placeholder()`: takes value from data passed to the `tf.session.run()` function, it can be used in conjunction with `feed_dict` parameter so you can change the input value before each session run. For example:

    ```python
    x = tf.placeholder(tf.string)
    
    with tf.Session() as sess:
        output = sess.run(x, feed_dict={x: 'Hello World'})
    ```

    In the above example, `x` is a placeholder, and the x is assigned value of `Hello World` before the run session.

  - `tf.Variable()`: creates a tensor with an initial value that can be modified, much like a normal Python variable. This tensor stores its state in the session, so you must initialize the state of the tensor manually. You'll use the [`tf.global_variables_initializer()`](https://www.tensorflow.org/programmers_guide/variables) function to initialize the state of all the Variable tensors. 

    ```python
    init = tf.global_variables_initializer()
    with tf.Session() as sess:
        sess.run(init)
    ```

## 3. Building a Classifier:

- **Linear Function:** TensorFlow uses nomenclature `y=xW+b` instead of `y=Wx+b` as we mentioned before, so note the the matrix has to be transposed.

  - Initializing the weights with random numbers from a normal distribution is good practice. Randomizing the weights helps the model from becoming stuck in the same place every time you train it.

    `weights = tf.Variable(tf.truncated_normal((n_features, n_labels)))`

  - For the biases, you don't need to randomize them, so it is good to just set the initial guess for biases to 0.

    `bias = tf.Variable(tf.zeros(n_labels))`

- **Softmax:** logits is the "score" for each label, and softmax function will turn the score into probabilities, below is one line of code to calculate softmax in python.

  ```python
  def softmax(x):
    return np.exp(x) / np.sum(np.exp(x), axis=0)
  ```

  In TensorFlow, it is even easier, just use:

  ```python
  x = tf.nn.softmax(x)
  ```

- **Training, Validation, and Test Set:** 

- **Stochastic Gradient Descent:** Instead of computing the "real" gradient of a large dataset and model (it can be very computationally in-efficient), use a very rough estimate: we only calculate a random subset of the data, compute the loss and derivative, and use those for gradient descent. It might result in us taking more steps, but the time consumed for each step is way less. This alternative method is called S.G.D. A few tricks for S.G.D:

  - Inputs: pre-conditioned so it has mean = 0 and small, equal variance.
  - Initial weights: random, mean = 0 and small, equal variance.
  - Momentum: keep a running average of the previous gradients
  - Learning Rate Decay: lowering the learning rate as iterations progresses
  - Always use small learning rate.

- **Mini Batching:** Mini-batching is a technique for training on subsets of the dataset instead of all the data at one time. This provides the ability to train a model, even if a computer lacks the memory to store the entire dataset.

- **Epoch:** An epoch is a single forward and backward pass of the whole dataset. 