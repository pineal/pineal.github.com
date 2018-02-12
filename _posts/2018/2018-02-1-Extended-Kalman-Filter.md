---
layout: post
title: Extended Kalman Filter
categories:
- Algorithm
- Robotics
tags:
- Robotics
- Signal Processing
---


## Review of Kalman filter

- [Previous post on basic kalman filter](http://pineal.github.io/2015/03/Kalman-Filter/])
- [iLecture lessons](http://www.ilectureonline.com/lectures/subject/SPECIAL%20TOPICS/26/190)

Distribution of Gausion is not Gaussian, it becomes non-linear

Extented Kalman filter uses a linear approximation of h(x)
Here we use first order taylor expansion to 

Given a function f(x), a taylor series expansion could be expressed:

$$f(x) \approx \frac{\partial{f(\mu)} }{\partial{x}}(x - \mu)$$


