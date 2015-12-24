---
layout: post
title: Optimal Control
categories:
- Robotics
tags:
- Control Theory
---
# Optimal Control

##Optimal Control Framework

- Given:
    - A controlled dynamical system：
    $$
    x^{n+1} = f(x^n, u^n)
    $$
    - A cost function：
    $$
    V = \phi(x^N, \alpha) + \sum^{N-1}_{i=0}L(x^i, u^i, \alpha)
    $$  

- Goal:
    -Find the sequence of commands that minimizes(maximizes) the cost function

##Bellman's Principle of Optimality
Optimize it using dynamic programming:
$$
J_i(X_i) = \mathop{arg min}_{u_i\in u(x_i)}\{{L(x^i, u^i, \alpha) + V^*_{i+1}x_{(i+1)}}\}
$$
##Linear quadratic regulator
- Special Assumption: Linear System Dynamics
$$
x^{n+1} = Ax^n + Bu^n
$$
- Quadratic cost function
$$
L(x^i, u^i, \alpha) ＝ x^{i^T}Qx^i + u^{i^T}Ru^{i^T}
$$

- Goal:
    - Bring the system to a setpoint and keep it there
    - Note: this an also be done with a nonlinear system by a local linearization


$$
  \begin{aligned}
 V^*_i(X_i) & = \mathop{arg min}_{u_i\in u(x_i)}\{{L(x^i, u^i, \alpha) + V^*_{i+1}x_{(i+1)}}\} \\
   & = \mathop{arg min}_{u_i\in u(x_i)}\{{x^{i^T}Qx^i + u^{i^T}Ru^{i^T} + V^*_{i+1}x_{(i+1)}}\} \\
   & = \mathop{arg min}_{u_i\in u(x_i)}\{{x^{i^T}Qx^i + u^{i^T}Ru^{i^T} + V^*_{i+1}(Ax^n + Bu^n)}\} \\
  \end{aligned}
$$

- As A linear control law expressed as:

$$
    u^{i^*} = -K^ix^i
$$
- Rewrite the optimal cost at stage i as a quadratic form:

$$
    {V^i}^* = {x^i}^TP^ix^i
$$

Therefore,
$$
   V^*_i(X_i) = \mathop{arg min}_{u_i\in u(x_i)}\{{x^{i^T}Qx^i + u^{i^T}Ru^{i^T} + V^*_{i+1}(Ax^n + Bu^n)}\} \\
$$

##Finite horizon approximation
##Motion Predictive Control
##Fast MPC
