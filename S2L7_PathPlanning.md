# Lesson 7~8: Search & Prediction

## Class Notes

### 1. Search Problem and Motion Planning

- First Search Program:
  - Expand to all possible adjacent neighbors, g-value increment for each expansion step.
  - See `L7U11_FirstSearch.py` for example.
- A* Search:
  - [Wikipedia page with a very nice writeup](https://en.wikipedia.org/wiki/A*_search_algorithm)
  - Heuristic function: pre-define a function to reflect the distance between each cell to the goal, `h(x, y) <= distance to goal`
  - When progressing in search, use the sum of g-value and heuristic function (f value), and only expand to the space with smaller f value.
  - See `L7U15_Astar_Search.py` for example.
- Dynamic Programming:
  - Similar to A*, DP solves route planning problem given a map and a goal position, however, it provides best path to goal position **from anywhere** on the map.
  - See `L7U22_LeftTurnPolicy.py` for example.

### 2. Prediction

- Inputs and Outputs:
  - The inputs to prediction module uses map and data from sensor fusion to know the location and motion of nearby objects.
  - The outputs contain possible trajectory (position and motion) of the objects and the corresponding probability.
- Approaches:
  - Model-based Prediction: incorporate our knowledge of physics, with constraints imposed by the map, road traffic, laws, etc. to make prediction and assign a probability.
    - Frenet Coordinates: The *s* coordinate represents distance *along* the road (also known as **longitudinal displacement**) and the *d* coordinate represents side-to-side position on the road (also known as **lateral displacement**).
    - Process Models:
      - Refer to paper titled ["A comparative study of multiple-model algorithms for maneuvering target tracking"](https://d17h27t6h515a5.cloudfront.net/topher/2017/June/5953fc34_a-comparative-study-of-multiple-model-algorithms-for-maneuvering-target-tracking/a-comparative-study-of-multiple-model-algorithms-for-maneuvering-target-tracking.pdf)
    - Classifying intent with multiple model algorithm
  - Data-driven Prediction: use training data to train ML model - this allows the algorithm to capture subtle patterns that may not be visible or known to human. There are two steps to this approach:
    - Offline training: feed some machine learning algorithm a lot of data to train it
    - Online prediction:
  - Hybrid Approaches:
    - Naive Bayes: simply combine a classifier with a filter - "blindly" or "naively" think all features contribute independently. The Gaussian Naive Bayes is all about:
      1. Select relevant featuers
      2. Identify the means and variances for different classes with training data.
      3. When predicting, calculate the conditional probabilities for each feature.
    - Gaussian Naive Bayes Classifier Excercise (refer to `L8U17_GaussianNaiveBayes.cpp`)
    - [Wikipedia article on Naive Bayes / GNB](https://en.wikipedia.org/wiki/Naive_Bayes_classifier#Gaussian_naive_Bayes)


  - 