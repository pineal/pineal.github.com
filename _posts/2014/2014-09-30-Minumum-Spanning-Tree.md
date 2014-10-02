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
##Copyright Statement
> The Exercise and some notes are homework or examples in course CSCI 570, computer science department, university of Southern California. All copyright belongs to the team of this course, including Professors and TAs. Posts on this blog are just for self-learning. 

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
The exercises are from homework of USC computer science.
###	Question 1

Consider two positively weighted graphs G = (V, E, w) and G' = (V, E, w') with the same vertices V and edges E such that, for any edge e ∈ E, we have w(e) = \\(w^2(e)\\).1. Prove or disprove: For any two vertices u, v ∈ V , any shortest path between u and v in GÕ is also a shortest path in G.
2. Prove or disprove: If T is a minimum spanning tree of G, then it must also be a minimum spanning tree for G'.
**Solution**
1. The claim is false. Consider the following counterexample. Let G and G' be undirected graphs with V = {a,b,c} and E = {(a,b),(b,c),(c,a)}. Define the weight function w by w(a,b) = 6,w(b,c) = 4, w(c, a) = 3 and consider the shortest path between nodes a and b. The edge (a, b) constitutes the shortest path in graph G, whereas the edge set {(a, c), (c, b)} constitutes the shortest path in graph G'.2. The claim is true. Consider an execution of Prim’s algorithm on G that led to the tree T. To see that the same execution sequence is valid on the graph G' we use the monotonicity of the \\((·)^2\\) function and proceed by induction. With the same starting node s in both cases, squaring the edge weights does not change the minimum weight edge and hence the first edge added to the partial tree is the same for both graphs G and GÕ. Now assume that the same set of edges have been added to the partial tree up to stage k on both graphs. So the edges to consider at stage k + 1, i.e. the edges that do not form a cycle, are exactly the same for both graphs. Among these candidate edges, squaring the edge weights does not change the minimum weight edge so that addition of the same edge is valid for both G and GÕ. Hence, the partial trees are the same for both graphs after stage k + 1 and the proof is complete by mathematical induction.
###Question 2
Suppose you are given a connected graph G, with edge costs that are all distinct. Prove that G has a unique minimum spanning tree.**Solution**
Suppose by way of contradiction that T and T' are two distinct minimum spanning trees for the graph. Since T and T' have the same number of edges but are not equal, there exists an edge e belongs T but not belongs to T'. adding e to T' results in a cycle C. As edge weights are distinct, take e' to be the uniquely most expensive edge in C. By the cycle property, no minimum spanning tree can contain e' contradicting the fact that the edge e' is in at least one of tree T or T'.
###Question 3Let us say that a graph G=(V,E) is a near-tree if it is connected and has at most n + 8 edges, where n = |V|. Give an algorithm with running time O(n) that takes a near-tree G with costs on its edges, and returns a minimum spanning tree of G. You may assume that all the edge costs are distinct.
**Solution**

The cycle property will be applied for 9 times. We perform BFS until we find a cycle in the graph G and then we delete the heaviest edge on this cycle. We have now reduced the number of edges in G by one, while keeping G connected and not changing the identity of the minimum spanning tree. If we do this 9 times, we will have a connected graph H with n-1 edges and the same minimum spanning tree as G. But H is a tree and thus it has to be the minimum spanning tree of G. The running time of each iteration is O(m+n) for the BFS and subsequent check of the cycle to find the heaviest edge. Since m≤n+8 and there are a total of nine iterations, total running time is O(n).###Question 4Consider the Minimum Spanning Tree Problem on an undirected graph G=(V,E), with a cost \\(c_e\\) ≥0 on each edge, where the costs may not all be different. If the costs are not all distinct, there can in general be many distinct minimum-cost solutions. Suppose we are given a spanning tree T ⊆ E with the guarantee that for every e ∈ T, e belongs to some minimum-cost spanning tree in G. Can we conclude that T itself must be a minimum-cost spanning tree in G? Give a proof or a counterexample with explanation.

**Solution**

Consider the following counterexample. Let G = (V, E) be a weighted graph with vertex set V = {v1, v2, v3, v4} and edges (v1, v2), (v2, v3), (v3, v4), (v4, v1) of cost 2 each and an edge (v1, v3) of cost 1. Then every edge belongs to some minimum spanning tree, but a spanning tree consisting of three edges of cost 2 would not be minimum.

###Question 5
Assume that you are given a graph G and a minimum spanning tree T of G. A new edge e is added to G (without introducing any other vertices) to create a new graph G ̄. Design an algorithm that given G, T and e, finds a minimum spanning for G ̄. Your algorithm should run in O(n) time.

**Solution**

We add the edge e to the minimum spanning tree T and this creates a unique simple cycle C. Let \\(e\_{max}\\) be an edge of maximum weight in the cycle C. Then, removing this edge would give us a spanning tree T'=T∪{e}{\\(e\_{max}\\)} since we would have |V|-1 edges and no cycles. To prove that T' is a minimum spanning tree for the new graph G', we consider two distinct cases.

1. w(e) = w(\\(e\_{max}\\))
2. w(e) < w(\\(e\_{max}\\))
Running Time: All we need to compute is the edge \\(e\_{max}\\). Construct the unique path (call it P) in T that connects the two ends of e. This can be accomplished using BFS in O(n) time. The cycle C is path P concatenated with edge e and we can compute \\(e\_{max}\\) as the maximum weighted edge in C.
###Question 6Let T be a minimum spanning tree for G with edge weights given by weight function w: E ∈ R. Choose one edge (x, y) ∈ T and a positive number k, and define the weight function w': E ∈ R by:
$$ f(x)=\cases{{w(u,v)}&{if (u,v)≠(x,y)}\\{w(u,v)}-k & {if (u,v)=(x,y)}} $$Show that T is also a minimum spanning tree for G with edge weights given by w'.
**Solution**

Consider an execution of Prim's algorithm on the Prim's algorithm on the graph G leading to the minimum spanning tree T, and examine what happens when the edge (x,y) is include. Without loss of generality, assume that the node x is included first in the partial tree T_p formed during this execution. Then the minimum weight edge incident on x that does not form a cycle with T_p in the edge (x,y) with weight w(x,y). Since the only difference between the graphs G and G' is the weight of the edge (x,y), the same execution of Prim's algorithm is valid for the graph G' up to the construction of the partial tree T_p, as the algorithm has not "seen" the edge (x,y) yet. Since k>0, w'(x,y) < w(x,y)  and so the minimum weight edge to be selected next by Prim's algorithm on the graph G' is again (x,y) (identical to the execution on G). After this step, the execution on graphs G and G' again proceeds identically and would lead to the same output since the edge(x,y) is never "seen" again by Prim's algorithm. Therefore, an execution of Prim's algorithm leads to the tree T on both graphs G and G' implying that T is also a minimum spanning tree for the graph G'.

Have no reason the github and my blog is done...
 
