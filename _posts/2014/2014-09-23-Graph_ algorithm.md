---
layout: post
title: Graph algorithm
categories:
- Algorithm
tags:
- graph
- BFS
- DFS
---
##Copyright Statement
> The Exercise and some notes are homework or examples in course CSCI 570, computer science department, university of California. All copyright belongs to the team of this course, including Professors and TAs. Posts on this blog are just for self-learning. 

##BFS and DFS
###Purpose of BFS, DFS
BFS and DFS are graph search algorithms that can be used for a variety of different purposes.

One common application of the two search techniques is to identify all nodes that are reachable from a given starting node. For example, suppose that you have a collection of computers that each are networked to a handful of other computers. By running a BFS or DFS from a given node, you will discover all other computers in the network that the original computer is capable of directly or indirectly talking to. These are the computers that come back marked.

BFS specifically can be used to find the shortest path between two nodes in an unweighted graph. Suppose, for example, that you want to send a packet from one computer in a network to another, and that the computers aren't directly connected to one another. Along what route should you send the packet to get it to arrive at the destination as quickly as possible? If you run a BFS and at each iteration have each node store a pointer to its "parent" node, you will end up finding route from the start node to each other node in the graph that minimizes the number of links that have to be traversed to reach the destination computer.

DFS is often used as a subroutine in more complex algorithms. For example, Tarjan's algorithm for computing strongly-connected components is based on depth-first search. Many optimizing compiler techniques run a DFS over an appropriately-constructed graph in order to determine in which order to apply a specific series of operations. Depth-first search can also be used in maze generation: by taking a grid of nodes and linking each node to its neighbors, you can construct a graph representing a grid. Running a random depth-first search over this graph then produces a maze that has exactly one solution.

This is by no means an exhaustive list. These algorithms have all sorts of applications, and as you start to explore more advanced algorithms you will often find yourself relying on DFS and BFS as building blocks. It's similar to sorting - sorting by itself isn't all that interesting, but being able to sort a list of values is enormously useful as a subroutine in more complex algorithms.

reference: 
<http://stackoverflow.com/questions/14720691/whats-the-purpose-of-bfs-dfs>

###Connectivity, DAG and topological Ordering

##Exercise
**1.** The police department in a city has made all streets one-way. The mayor contends that there is still a way to drive legally from any intersection in the city to any other intersection, but the opposition is not convinced. A computer program is needed to determine whether the mayor is right. However, the city elections are coming up soon, and there is just enough time to run a linear-time algorithm.

- Formulate this as a graph problem and design a linear-time algorithm. Explain why it can be solved in linear time.
- Suppose it now turns out that the mayor’s original claim is false. She next makes the following claim to supporters gathered in the Town Hall: ”If you start driving from the Town Hall (located at an inter- section), navigating one-way streets, then no matter where you reach, there is always a way to drive legally back to the Town Hall.” Formulate this claim as a graph problem, and show how it can also be verified in linear time.
 
**Solution:** 

- The mayor is merely contending that the graph of the city is a strongly-connected graph, denoted as G. Form a graph of the city(intersections becomes nodes, one-way street become directed edges). Suppose there are n nodes and m edges. The algorithm is:
 
 1. Choose an arbitrary node s in G, and run BFS from s. If all the nodes are reached the BFS tree rooted at s, then go to step 2. Otherwise the mayor's statement must be wrong. This operation takes time O(m+n).
 2. Reverse the direction of all the edges to get the graph \\(G^inv\\), this step takes time O(m+n).
 3. Do BFS in \\(G^inv\\) starting from s. If all the nodes are reached, then the mayor's statement is correct; otherwise, the mayor's statement is wrong. This step takes time O(m+n).
 
Explanation: BFS on G tests if s can reach all other nodes; BFS on Ginv tests if all other nodes reach s. If these two conditions are not satisfied, the mayor’s statement is wrong obviously; if these two conditions are satisfied, any node v can reach any node u by going through s.- Now the mayor is contending that the intersections which are reachable from the city form a strongly-connected component. Run the first step of the previous algorithm while setting the Town Hall as s(test to see which nodes are reachable from Town Hall). Remove any nodes which are unreachable from Town Hall, and this is the component which the mayor is claiming is strongly connected. Run the previous algorithm on this component to verify it is strongly connected. 
**2.** The algorithm described in Section 3.6 for computing a topological ordering of a DAG repeatedly finds a node with no incoming edges and deletes it. This will eventually produce a topological ordering, provided that the input graph really is a DAG.
But suppose that we’re given an arbitrary graph that may or may not be a DAG. Extend the topological ordering algorithm so that, given an input directed graph G, it outputs one of two things: (a) a topological ordering, thus establishing that G is a DAG; or (b) a cycle in G, thus establishing that G is not a DAG. The running time of your algorithm should be O(m + n) for a directed graph with n nodes and m edges.
**Soultion:**

- Run the topology ordering algorithm for graph G. Try to delete a node with no incoming edge from whole graph G at each iteration. If at a certain iteration, all the remaining nodes have at least one incoming edge, we can confirm that G must contain a circle. We output the remaining graph as \\(G^'\\). Otherwise, the algorithm must find a topological ordering for whole graph G, returning G as DAG. The complexity is O(m + n).
- In the case of \\(G^'\\), reverse the edges in \\(G^'\\) to get \\(G^'inv\\) to guarantee that all the nodes in \\(G^'inv\\) has at least one outgoing edge. The reversing procedure takes O(m) because there is no isolated node in \\(G^'\\).
- ???Start traversing from an arbitrary vertex v in \\(G^'inv\\) along the outgoing edges until we revisit certain node, say node u. This procedure takes no more than n steps. During this procedure, we need to record the predecessor of each visited node. After the traversing procedure stops, we just trace back from u according to the predecessor records and return the loop we found. The traversing and tracing back procedure takes O(n) time.
- The total complexity is O(m + n).