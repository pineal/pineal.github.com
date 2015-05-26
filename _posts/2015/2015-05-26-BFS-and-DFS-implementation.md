---
layout: post
title: BFS and DFS implementation
categories:
- Algorithm
tags:
- graph
- BFS
- DFS
---


##BFS implementation

```
BFS(s):
Set Discovered[s] = true and Discovered[v] = false for all other v
Initialize L[0] to consist of the single element s
Set the layer counter i=0
Set the current BFS tree T = ∅
While L[i] is not empty
  Initialize an empty list L[i+1]
  For each node u ∈ L[i]
    Consider each edge (u,v) incident to u
    If Discovered[v] = false then
      Set Discovered[v] = true
      Add edge (u,v) to the tree T
      Add v to the list L[i+1]
    Endif
  Endfor
  Increment the layer counter i by one
Endwhile

```
The above implementation of the BFS algorithm runs in time O(m + n) (i.e., linear in the input size), if the graph is given by the adjacency list representation.




##DFS implementation

```
DFS(s):
  Initialize S to be a stack with one element s
  While S is not empty
    Take a node u from S
    If Explored[u] = false then
      Set Explored[u] = true
      For each edge (u,v) incident to u
        Add v to the stack S
      Endfor
    Endif
  Endwhile
```

The above algorithm implements DFS, in the sense that it visits the nodes in exactly the same order as the recursive DFS procedure in the previous section (except that each adjacency list is processed in reverse order).

The above implementation of the DFS algorithm runs in time O(m + n) (i.e., linear in the input size), if the graph is given by the adjacency list representation.
