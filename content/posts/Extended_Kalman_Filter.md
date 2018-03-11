---
layout: post
title: Extended Kalman Filter
categories:
- Algorithm
- Robotics
tags:
- Robotics
- Math
date : "2018-02-01T00:00:00"
---


## Review of Kalman filter

- [Previous post on basic kalman filter](http://pineal.github.io/2015/03/Kalman-Filter/])
- [iLecture lessons](http://www.ilectureonline.com/lectures/subject/SPECIAL%20TOPICS/26/190)

Distribution of Gausion is not Gaussian, it becomes non-linear

Extented Kalman filter uses a linear approximation of h(x)
Here we use first order taylor expansion to 

Given a function f(x), a taylor series expansion could be expressed:

$$f(x) \approx \frac{\partial{f(\mu)} }{\partial{x}}(x - \mu)$$

## Multivariate Taylor Series


## Design Kalman Filter for 1D tracking problem

We need to define two linear functions:
1. state transition function
2. measurement function

### State transition function   

$$ x' = F * x + noise $$

where, 

$$F = \begin{pmatrix} 
        1 & \Delta{t} \\
        0 & 1
     \end{pmatrix}$$

$$x = \begin{pmatrix} p \\ v\end{pmatrix}$$

postion $p$ is linear motion model, calculation is:

$$p' = p + v * \Delta{t}$$

Thus We can express it in a matrix form:

$$
\begin{pmatrix} p' \\ v' \end{pmatrix}
=
\begin{pmatrix} 
        1 & \Delta{t} \\
        0 & 1
     \end{pmatrix}
\begin{pmatrix} p \\ v\end{pmatrix}
$$

### Measurement Update function


