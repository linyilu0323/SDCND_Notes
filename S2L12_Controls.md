# Lesson 12: Controls

## Class Notes

### 1. PID Control

- Proportional (P) Control: 
  - `Control Effort = Gain_P * Error`
  - A single P controller would usually result in "overshoot" and subsequent oscillation.
- Proportional-Differential (PD) Control:
  - `Control Effort = Gain_P * Error + Gain_D * d/dt(Error)`
  - Reduces oscillation but response is slower. It cannot solve "systematic error" where system introduces intrinsic error that is beyond (or downstream of) controller.
- Proportional-Integral-Differential (PID) Control:
  - `Control Effort = Gain_P * Error + Gain_D * d/dt(Error) + Gain_I * int(Error *dt)`


### 2. Parameter Optimization

- The gain factor for PID controller is important as it results in different system response (fast/slow, overshoot, static error, etc.).

- Twiddle: an parameter optimization algorithm to tune the PID gain parameters

  ```python
  # Choose an initialization parameter vector
  p = [0, 0, 0] # corresponds to Gain_P, Gain_I, Gain_D
  # Define potential changes
  dp = [1, 1, 1]
  # Calculate the error
  best_err = A(p) # A is the system function
  
  threshold = 0.001
  
  while sum(dp) > threshold:
      for i in range(len(p)):
          p[i] += dp[i]
          err = A(p)
  
          if err < best_err:  # There was some improvement
              best_err = err
              dp[i] *= 1.1
          else:  # There was no improvement
              p[i] -= 2 * dp[i]  # Go into the other direction
              err = A(p)
  
              if err < best_err:  # There was an improvement
                  best_err = err
                  dp[i] *= 1.1
              else:  # There was no improvement
                  p[i] += dp[i]
                  # As there was no improvement, the step size in either
                  # direction, the step size might simply be too big.
                  dp[i] *= 0.9
  ```

### 3. Other Control Methods

- Model Predictive Control (MPC)  [Vision-Based High Speed Driving with a Deep Dynamic Observer](https://arxiv.org/abs/1812.02071) by P. Drews, et. al.
- Reinforcement Learning-based [Reinforcement Learning and Deep Learning based Lateral Control for Autonomous Driving](https://arxiv.org/abs/1810.12778) by D. Li, et. al.
- Behavioral Cloning [ChauffeurNet: Learning to Drive by Imitating the Best and Synthesizing the Worst](https://arxiv.org/abs/1812.03079) by M. Bansal, A. Krizhevsky and A. Ogale

