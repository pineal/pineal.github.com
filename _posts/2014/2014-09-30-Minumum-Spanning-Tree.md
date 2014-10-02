---
layout: post
title: Minimum Spanning Tree
categories:
- Algorithm
tags:
- Minimum Spanning Tree
- Kruskal's Algorithm
- Prim's Algorithm
- Reverse-Delete algorithm
- Union-Find data Structure
---

##Concepts and Definition
###Kruskal's algorithm
Sort all edges in increasing order of cost. Add edges to T in this order as long as it does not create a cycle. 

###Prim's algorithm
Start with a node s and place it in set S. At each step grow S by one node each time adding the node v that minimizes attachment cost (between S and V-S ).

###Reverse-delete
Start with the full graph, begin deleting edges in order of decreasing cost as long as it does not disconnect the graph.

###Cut Property 
The crucial fact about edge insertion to guarantee safety to include an edge in the minimum spanning tree is called cut property.

> Assume that all edge costs are distinct. Let S be any subset of nodes that is neither empty nor equal to all of V, and let edge e = (v, w) be the minimum cost edge with one end in S and the other in V-S. Then every minimum spanning tree contains the edge S.

###Cycle Property 
The crucial fact to guarantee an edge is not in the minimum spanning tree is called cycle property. 

> Assume that all edge costs are distinct. Let C be any cycle in G, and let edge e=(v,w) be the most expensive edge belonging to C. Then e does not belong to any minimum spanning tree of G.

Any algorithm that builds a spanning tree by repeatedly including edge when justified by the Cut Property and deleting edges when justified by the Cycle Property - in any order at all - will end up with a minimum spanning tree.

##Implementation
###Kruskal's algorithm

```
A=NULL
Create an independent set for each node
Sort the edges of E into non-decreasing order of cost
For each edge (u,v) ∈ E taken in this order 
	if u and v are not in the same set
		Merge the two set
		A = A ∪ {u, v}
	Endif
Endfor
```	

###Union-Find Data Structure

- **Makeset**(construct)**:** O(1) for set size = 1

- **FindSet:** O(1)*(array)* or O(lgn)*(pointer)*

-  **Union(merge):** O(1)*(array)* or O(lgn)*(pointer)*

```
A=NULL
for each vertex v ∈ V
	Makeset(v)
endfor
sort the edges of E into non-decreasing order of cost
for each edge (u,v)∈E taken in this order
	if FindSet(u)≠FindSet(v)
	Union(v,u)
	A=A∪{(u,v)}
	endif
endfor
```

###Complexity of Kruskal's
- Makeset: O(n)
- sort: O(m log m)
- loop takes: O(m log n)
- overall cost: O(m log m)

###Complexity of Prim's
- Relaxation step for Dijkstra's: decrease-key (Q,u,d(v)+le)
- Relaxation step for Prim's: decrease-key (Q,u,le)

Take binary heap for example, time complexity for both Dijkstra's and Prim's is O(m log n). Let us assume that for a very dense graph m=n^2, then for Kruskal's and Prim's the time complexity is same: O(m log n).

###Complexity of Reverse-delete
O(m) * O(m+n) = O(m^2)

##Exercises
