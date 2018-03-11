---
layout: post
title: Linear Programming
categories:
- Algorithm
tags:
- Linear Programming
date : "2014-11-26T00:00:00"
---

$$\matrix{A} \times \matrix{X} = \matrix{B}  $$

Coeffienent matrix

A => n * n, x vector of unknowns, B right hand side

Linear system of equations.

_____

$$A \times X ≥ B$$

Objective function

$$C^T \times X $$

linear

Goal : Minimize the objective function

E.g.:
$$
\cases{x1 - x2 ≥ 0 \\\\
x_1 ≥ 0	\\\\
x_2 ≥ 0	\\\\
x1 + x2 ≤ 4
}$$

Maximize
$x1 + 2x_2$

###Simplex method
Only look at the vertex

Start of one vertex, go clockwise, find the max before the value going down

###Weighted vertex cover problem

for G = (V, E), S $\in$ V in a set such that each edge has at least one end in S $W_i ≥ 0$ for each i $\in$ V $$W(S) = \sum i \in S w_i$$

Objective: Minimize W(S)


Model this as an LP

x_i is a decision valuable for each node i \in V


$$
\cases{
x_1 - x_2 ≥ 0 \\\\
x_1 ≥ 0	\\\\
x_2 ≥ 0	\\\\
x_1 + x_2 ≤ 4
}$$

$$
\cases{
x_i = 0 & i \notin S \\\\
x_i = 1 & i \in S
}
$$

x_i + x_j ≥ 1 for each edge


Minimize \sigma w_ix_i
Subject to
$x_i + x_j ≥ 1 for (i, j) \in E$
$x_i \in {0,1} for i \in V$  <= discreate

Integer Programming

Linear programming	(continues variables)
Integer Programming	(discrete variables)
mixed integer programming



Drop the requirement that x_i \in {0,1}
and solve the LP in poly time and find {x_i^*} between 0 and 1

so that x_i^* + x_j^* ≥ 1 for each edge

W_{LP} = \sigma w_ix_i^*

S^* is the opt vertex cover set
W(S^*) = weight of the opt solution

W(S^*) ≥ W_{LP}

S is our approx. solution

W(S) ≤ 2 * W_{LP}
W_{LP} ≤ 2 * W(s^*)

W(S) ≤ 2 * W(S^*)

Solve the max. flow problem using LP. Variable are flow over edges.

Maximize \sigma f{e}
subject to
0 ≤ f(e) ≤ c_e foe each edge e \in E
\sigma f(e) - \sigma f(e) = 0 for v \in V


A = B
A - B ≥ 0
B - A ≥ 0

Max flow with lower bounds on flow over the edges
objective function stays same
conservation of the flow stays same

Cap constraint: l_e≤f(e)≤c_e for each edge e \in E

###Multi commodity flow
f_i(e): flow of commodity i over edge e
\alpha_i: is the profit associated with one unit of flow for commodity i.

We have m commodities

Objective: maximize profit

Maximize $\sigma_{l=i}^m \sigma_{eoutofS} \alpha_i f_i(e)$

subject to 0 ≤ \sigma_{i=1}^m f_i{e} ≤c_{e} for each e \in E

\sigma_{i=1}^m f_i{e} = \sigma_{i=1}^m f_i{e} for each node v \in V and for each i = 1 to m

###Shortest path using LP
Shortest distance from V to t is d(v) for each node V
For each node V
$$
d(t) ≤ d(y) + c_{yt} \\\\
d(t) ≤ d(w) + c_{wt} \\\\
d(t) ≤ d(x) + c_{xt} \\\\
$$

d_{v} ≤ d_{u} + w(u, v) for each edge (u, v) \in E
d(s) = 0

Objective function:
	Minimize d(t)

1:36
