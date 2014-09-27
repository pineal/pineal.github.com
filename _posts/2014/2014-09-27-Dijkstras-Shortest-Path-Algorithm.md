---
layout: post
title: Dijkstras Shortest Path Algorithm
categories:
- Algorithm
tags:
- Dijkstras
---

##Copyright Statement
> The Exercise and some notes are homework or examples in course CSCI 570, computer science department, university of SoutCalifornia. All copyright belongs to the team of this course, including Professors and TAs. Posts on this blog are just for self-learning. 

##Problem Formulation
Given g=(V,E) with wights w(u,v)≥0 for each edge (u,v)∈E find the shortest path from s∈V to V-S.


##Algorithm Design
The algorithm can be designed in following procedure:

1. Start with a set S of vertices whose final shortest path we already know.
2. At each step find a vertex u∈V-S with shortest distance from s.
3. Add u to S, repeat.

##Implementation
It can also be represented by the pseudocode in following:

```
Dijkstra's Algorithm (G,l)
Let S be the set of explored nodes
	For each u∈S, we store a distance d(u)
Initially S = {s} and d(s) = 0
While S ≠ V
	Select a node v∉S with at least one edge from S for which 
		d(v)= min(d(u) + le)e=(u,v):u∈S is as small as possible
	Add v to S and define d(v) = d'(v)
Endwhile
```

##Demonstration
![ExampleProblem](http://img.blog.163.com/photo/WfZavQsDv8XkOKX7DnpGUg==/583497626721859365.jpg)

![ExampleSolution](http://img.blog.163.com/photo/FigAtAu_VN_237zxziOrmw==/5113555901903496072.jpg)