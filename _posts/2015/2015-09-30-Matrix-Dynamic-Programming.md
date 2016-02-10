---
layout: post
title: Matrix Dynamic Programming
categories:
- Algorithm
tags:
- dynamic programming
---
## Questions
- Unique Path
- Min Path Sum

一般这类问题都是给了一个m＊n的矩阵(或者三角形)，从一个方向走到另一个方向（从顶到底，或者从左上角到右下角 .etc），一般来说只会朝固定的方向走。

初始化两条边，然后根据条件循环更新状态方程，主要看上一步的状态，最后得出结果。
