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

##Implementation
The implementation will base on priority queue(Heaps).

```
S=Null
Initialize priority queue Q with all nodes V. With d(v) as their key value (all d(v)'s=infinity except for S where d(s)=0)
While S ≠ V
	V = ExtractMin(Q)
	S=S∪{v}
	for each vertex u∈Adj(v)
		if d(u) > d(v) + le:
			decrease-key(Q,u,d(v)+le)
	endfor
endwhile
```
> decrease-key(x, k, H): Replace the key of item x in heap H by k, which is smaller than thecurrent key of x.

###Complexity Analysis
O(n) for the initialization
while loop O(n)
for loop O(n)

> Dense graph is a graph in which the number of edges is close to the maximal number of edges. Sparse graph is a graph in which the number of edges is close to the minimal number of edges. Sparse graph can be a disconnected graph.


n extractMin's
m decreaseKey's

|Operation/Heap | Binary Heap | Binomial Heap | Fibonacci Heap|
|------------ |------------ | ------------- | ------------|
|Cost of n extractMins		| nlogn | nlgn   | nlgn   |
|Cost of m decreaseKeys 	| mlogn | mlgn   | m      |
|Total				 		| mlogn | mlogn  | nlgn+m |
|Sparse Graph				| nlgn  | nlgn   | nlgn   |
|Dense Graph				| n^2lgn| n^2lgn | n^2    |

##Example
###Question
Hardy decides to start running to work in SF city to get in shoe. He prefers a route that goes entirely uphill and then entirely downhill so that he could work up a sweat uphill and get a nice, cool grease at the end of his run as he runs faster downhill. He starts running from his home and ends at his workplace. To guide his run, he prints out a detailed map of the road between home and work with k intersections and m road segments(any existing road between two intersections). Each intersection has a distinct elevation. Assuming that every road segment is either fully uphill or fully downhill, give an efficient algorithm to find the shortest path that meets Hardy's specifications. If no such path meets Hardy's specifications, your algorithm should determine this. Justify your answer. 

###Solution
1. Remove all down hill edges
2. Find shortest path to all nodes in the graph starting from home
3. Find shortest path to all nodes in the graph staring from workspace
4. add up shortest distance from home and to workspace and pick node with lowest total cost/distance.
5. If all costs are infinity, then there is no such path.

Cost of this solution is same as Dijkstra's algorithm