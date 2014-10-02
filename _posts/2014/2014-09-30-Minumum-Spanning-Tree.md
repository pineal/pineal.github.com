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


###Kruskal's algorithm
Sort all edges in increasing order of cost. Add edges to T in this order as long as it does not create a cycle. 

###Prim's algorithm
Start with a node s and place it in set S. At each step grow S by one node each time adding the node v that minimizes attachment cost (between S & V-S ).

###Reverse-delete
Start with the full graph, begin deleting edges in order of decreasing cost as long as it does not disconnect the graph.

The crucial fact about edge insertion to guarantee safety to include an edge in the minimum spanning tree is called cut property.

> Assume that all edge costs are distinct. Let S be any subset of nodes that is neither empty nor equal to all of V, and let edge e = (v, w) be the minimum cost edge with one end in S and the other in V-S. Then every minimum spanning tree contains the edge S.

###Cycle Property 
The crucial fact to guarantee an edge is not in the minimum spanning tree is called cycle property. 

> Assume that all edge costs are distinct. Let C be any cycle in G, and let edge e=(v,w) be the most expensive edge belonging to C. Then e does not belong to any minimum spanning tree of G.

Any algorithm that builds a spanning tree by repeatedly including edge when justified by the Cut Property and deleting edges when justified by the Cycle Property - in any order at all - will end up with a minimum spanning tree.