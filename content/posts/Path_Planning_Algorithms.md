---
layout: post
title: Dijkstra's Shortest Path Algorithm
date : "2014-09-27T00:00:00"

draft: true
categories:
- Algorithm
tags:
- Dijkstras
- Greedy
---

## Problem Formulation
Given g=(V,E) with wights w(u,v)≥0 for each edge (u,v)∈E find the shortest path from s∈V to V-S.


## Algorithm Design

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

## Demonstration
![ExampleProblem](http://img.blog.163.com/photo/WfZavQsDv8XkOKX7DnpGUg==/583497626721859365.jpg)

![ExampleSolution](http://img.blog.163.com/photo/FigAtAu_VN_237zxziOrmw==/5113555901903496072.jpg)

## Implementation
The implementation will base on priority queue(Heaps).

```
S=Null
Initialize priority queue Q with all nodes V. With d(v) as their key value (all d(v)'s=infinity except for s where d(s)=0)
While S ≠ V
	v = ExtractMin(Q)
	S=S∪{v}
	for each vertex u∈Adj(v)
		if d(u) > d(v) + le:
			decrease-key(Q,u,d(v)+le)
	endfor
endwhile
```

> decrease-key(x, k, H): Replace the key of item x in heap H by k, which is smaller than the
current key of x.

### Complexity Analysis
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

## Example

### Question 1

Hardy decides to start running to work in SF city to get in shoe. He prefers a route that goes entirely uphill and then entirely downhill so that he could work up a sweat uphill and get a nice, cool grease at the end of his run as he runs faster downhill. He starts running from his home and ends at his workplace. To guide his run, he prints out a detailed map of the road between home and work with k intersections and m road segments(any existing road between two intersections). Each intersection has a distinct elevation. Assuming that every road segment is either fully uphill or fully downhill, give an efficient algorithm to find the shortest path that meets Hardy's specifications. If no such path meets Hardy's specifications, your algorithm should determine this. Justify your answer.

### Solution

1. Remove all down hill edges
2. Find shortest path to all nodes in the graph starting from home
3. Find shortest path to all nodes in the graph staring from workspace
4. add up shortest distance from home and to workspace and pick node with lowest total cost/distance.
5. If all costs are infinity, then there is no such path.

Cost of this solution is same as Dijkstra's algorithm

### Question 2
Consider the following modification to Dijkstra’s algorithm for single source shortest paths to make it applicable to directed graphs with negative edge lengths. If the minimum edge length in the graph is −w < 0, then add w + 1 to each edge length thereby making all the edge lengths positive. Now apply Dijkstra’s algorithm starting from the source s and output the shortest paths to every other vertex. Does this modification work? Either prove that it correctly finds the shortest path starting from s to every vertex or give a counter example where it fails.

### Solution
1. No, the modification does not work. Consider the following directed graph with edge set {(a, b), (b, c), (a, c)} all of whose edge weights are −1. The shortest path from a to c is {(a, b), (b, c)} with path cost −2. In this case, w = 1 and if w + 1 = 2 is added to every edge length before running Dijkstra’s algorithm starting from a, then the algorithm would output (a, c) as the shortest path from a to c which is incorrect.

2. Another way to reason why the modification does not work is that if there were a directed cycle in the graph (reachable from the chosen source) whose total weight is negative, then the shortest path is not well defined since you could traverse the negative cycle multiple times and make the length arbitrarily small. Under the modification, all edge lengths are positive and hence clearly the shortest paths in the original graph are not preserved.

###	Question 3
You are given a weighted directed graph G = (V, E, w) and the shortest path distances δ(s, u) from a source vertex s to every other vertex in G. However, you are not given π(u) (the predecessor pointers). With this information, give an algorithm to find a shortest path from s to a given vertex t in O(|V | + |E|) time.

### Solution 1

1. Starting from node t, go through the incoming edges to t one at a time to check if the condition δ(s, t) = w(u, t) + δ(s, u) is satisfied for some (u, t) ∈ E.
2. There must be at least one node u satisfying the above condition, and this node u must be a predecessor of node t.
3. Repeat this check recursively, i.e. repeat the first step assuming node u was the destination instead of node t to get the predecessor to node u, until node s is reached.

Complexity Analysis: Since each directed edge is checked at most once, the complexity is O(|V | + |E|). Note that constructing the adjacent list of \\(G^{rev}\\) in order to facilitate access to the incoming edges is needed, which is still bounded by O(|V | + |E|) complexity.

### Solution 2
Run a modified version of Dijkstra’s algorithm without using the priority queue.

```
Set S = {s}
While t ̸∈ S
	For each node u satisfying u ̸∈ S, (v,u) ∈ E and v ∈ S, check if the condition δ(s,u) = w(v, u) + δ(s, v) is true.
	If true, add u to S, and v is the predecessor of u.
Endwhile
```

Complexity Analysis: Since each directed edge is checked at most once, the complexity is O(|V | + |E|).

### Question 4
We are given a directed graph G = (V, E) on which each edge (u, v) ∈ E has an associated value r(u, v), which is a real number in the range 0 ≤ r(u, v) ≤ 1 that represents the reliability of a communication channel from vertex u to vertex v. We interpret r(u, v) as the probability that the channel from u to v will not fail, and we assume that these probabilities are independent. Give an efficient algorithm to find the most reliable path between any two given vertices.

1. Assign the edge weight w(u, v) = − log r(u, v) to each edge (u, v) ∈ E, with appropriate representation for edge weight of infinity.
2. Run Dijkstra’s shortest path algorithm and return the path found from node s to node t as the path P∗.

Since r(u,v) ∈ [0,1], all weights w(u,v) are non-negative and hence Dijkstra’s algorithm gives the
correct shortest path, and thus also the most reliable path connecting node s to node t.
