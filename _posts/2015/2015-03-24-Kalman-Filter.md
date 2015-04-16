---
layout: post
title: Kalman Filter
categories:
- Algorithm
tags:
- Robotics
- Signal Processing
---
##Purpose and Usage
- Eliminate noise in measurements
- Generate non-observable states(e.g., Velocity from position signals)
- For prediction of future state
- Optimal filtering

##Framework and Model
###Given:
- A discrete stochastic linear controlled dynamical system
  $$x_k = Ax_{k-1} + Bu_{k-1} + w_{k-1}$$

Each current signal value x^k is a combination of previous signal value $x_{k-1}$ times a constant, a control signal $u_{k}$ and a process noise and a process noise signal $w_{k-1}$(which usually considered as zero).

- A measurement function
  $$ y_{k} = Hx_{k} + v_{k} $$
Where $v_{k}$ is the measurement noise.

- Assume the process noise and the measurement noise are both considered to be normal distribution that

    $$ p(w) ∼ N (0, Q), $$
    $$ p(v) ∼ N (0, R). $$
In reality, covariance matrix Q and R may change in every iteration. We assume they are constant here however.


###Goal:
Find the best (recursive) estimate of the state x of the system.

###Computational Origins
Define $e_{k}^{-}$ to be a priori state estimate at step k given knowledge of the process prior to step $k$, and define $e_{k}$  to be a posteriori state estimate at step $k$ given measurement $z_{k}$. Then a priori and a posteriori estimate errors can be defined as:

  $$e_{k}^{-} ≡ x_{k} - \hat{x}_{k}^{-}$$

  $$e_{k} ≡ x_{k} - \hat{x}_{k}$$

The a priori estimate error covariance is then

  $$P_{k}^{-} = E[e_{k}^{-}e_{k}^{-T}]$$

The a posteriori estimate error covariance is then

  $$P_{k} = E[e_{k}e_{k}^{T}]$$

Then How can we optimally (linearly) combine the estimate and measurement to obtain the best reconstruction of ￼the true x? The answer given in “The Probabilistic Origins of the Filter” found.

$$   \hat{x} = \hat{x}_{k}^{-} - K \times residual $$

Where *residual* is $z_k - H \hat{x}_{k}^{-}$. It also can be called as measurement innovation.

The Kalman filter gains are derived by minimizing the posterior error covariance, resulting in

$$K_k = \frac{P_k^-H^T}{(HP_k^- + R)^{-1}}$$


If the a priori estimate of the process noise is zero, Then
  $$K = 0$$

If the measurement noise is zero
  $$K = H^{-1}$$
