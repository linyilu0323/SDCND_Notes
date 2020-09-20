# Lesson 10: Neural Networks

## Class Notes

### 1. Intuitions and Concepts

- **Classification Problem:** think about the very simple prediction problem, where we use two inputs (grades and test score), and one output (acceptance result) to try predicting if a new data point with given grades and test score can get accepted. 
- **Perceptron:** Data, like test scores and grades, are fed into a network of interconnected nodes. These individual nodes are called [perceptrons](https://en.wikipedia.org/wiki/Perceptron), or artificial neurons, and they are the basic unit of a neural network.
  - **Weights (W):** When input comes into a perceptron, it gets multiplied by a weight value that is assigned to this particular input. A higher weight means the neural network considers that input more important than other inputs. 
  - **Bias (b):** A bias, lets us move values in one direction or another. 
  - **Activation Function: ** The results of the perceptron's summation is turned into an output, this is done by feeding the linear combination into an activation function.
- **Logical Operator for Perceptrons:** Imagine if you're building an "AND" operator, in the perceptron terminology, one way is to set both weights to 1.0, and then the bias to -1.0. This way, only when both inputs are true, will we get a positive output.

## 2. Continous Predictions:

  - **Softmax Function:** Given score for each linear function, return the probability of each function to be the exponential of the score normalized by summation of exponential of the score of each function.
  - **One-Hot Encoding:** is a way to represent categorical data, one-hot means the values are always with a single high (1) bit and all the others low (0).
  - **Maximum Likelihood:** 
  - **Cross-Entropy:** to make life easier for "probability" calculation, which is a product, we take logarithm of the probability, which then becomes a sum of logarithm of each individual probability. For better intuition, we take the negative value of log result (as probability always between 0~1, then the log result is always <0). With a higher cross-entropy, the combination is less likely to happen.

- **Minimizing Error Function: Gradient Descent **

  - Start with a random parameter for `W` and `b` 
  - The error function `E(w,b) = -1/m * Sum(yi * ln(yi_pred) + (1-yi) * ln(1-yi_pred))` where the `y_pred` is given by `y_pred = act_func(W* xi + b)`
  - The gradient of error function would be `Grad E = - (y - y_pred) (x1, x2, ..., xn)`
  - Update the parameter weight `W` by `Wi_new = Wi + alpha * (y - y_pred) * xi`
  - Update the parameter bias `b` by `b_new = b + alpha * (y - y_pred)`
  

## 3. Neural Network Architecture:

- **Combining Models:** since a model is nothing but simply a probability space, we can mathematically combine two models; similar to before when we deal with linear models, we can also use weights and biases to manipulate how do we want to combine the models.
- **Feedforward:** Feedforward is the process neural networks use to turn the input into an output. It is simply the calculation of probability (or prediction) with the current model parameter (W and b).
- **Backpropagation:** In a nutshell, backpropagation will consist of:
  - Doing a feedforward operation.
  - Comparing the output of the model with the desired output.
  - Calculating the error.
  - Running the feedforward operation backwards (backpropagation) to spread the error to each of the weights.
    - `W_ij_k_new = W_ij_k - learn_rate * dE/dWij_k`
  - Use this to update the weights, and get a better model.
  - Continue this until we have a model that is good.
- **Chain Rule:** This is how we update the weights and biases for a multi-layer neural network.
  - For any function f, g, if `A = f(x), B = g(A) = g(f(x))`, then the derivative of B on x is: `dB/dA * dA/dx`.
  - The above equation for "chain rule" is especially helpful for multi-later neural networks, because the feedforward is the composition of multiple functions, and back-propagation is calculating the derivative of the composition functions.