
---
layout: post
title: NP Problem
categories:
- Algorithm
tags:
- NP problem
---


##NP problem


plan: explore the space of computationally hand problem to arrive at a mathematic characteristic a large d of them

Technique: compare negative difficultly of different problem


Loose definition:
If problem X in at least as hard as problem Y. It means that if we could solve X, we could also solve Y.

Y≤p X Y is polynomial time reduced to X
if Y can be solved using a poly number of std computational steps plus a poly no of called to a black box to love X.

Fact: 

Suppose Y ≤P X. If X can be solved in polynomial time, then Y can be solved in polynomial time.

Suppose Y ≤P X. If Y cannot be solved in polynomial time, then X cannot be solved in polynomial time.


Independent Set
Ref: in a graph G = (V,E), we say a set of nodes, s \in V in "independent" if no two nodes in S and joined by an edge. 


Find the maximum size independent set in graph G.(Optimization version)
Given a graph G and a number k, does G contain an independent set of size at least K.(Decision version)
No solution.

Vertex Cover
Given a graph G=(V,E) we say that a set of nodes S \in V in a vertex cover if every edge e \in E has at least one end in S.


Find the smallest vertex cover set in G (Optimization version)
Given a graph G=(V,E) and a number k, G contain a vertex cover of size at most k.(Decision version)



FACT: let G=(V,E) be a graph then S in an independent set if and only if its complement (V-S) is a vertex cover. 

S is an independent set. 

case 1 
U is in S and V is not. 
V-S will have V and not U. 

case 2
V is in S and U is not.
V-S will have U and not V

case 3 
Neither V or U are in the set

Suppose V-S is a vertex cover can prove that S is an independent set. 

Claim: independent Set≤P vertex Cover
Prove If we have a black box to solve the vertex cover problem. We can decide if G has independent set of size at least k by asking the black box if G has a vertex cover of size at most n-k.

Claim: Vertex cover ≤S independent Set

Proof: if we have a black box to solve the independent set we can deicide if G has a vertex cover of size at most k by asking the black box if G has an independent set of size at least n-k.


Set cover problem
Given a set U of n elements, a collection s_1 to s_m of sub sets of U and and a number of k, does there exist a collection of at most k of these sets whose union is equal to all of U.

Set U = set of all edges in G
S1 = {(1, 2), (1,3)}
S2 = {(1,2), (2,3), (2,4), (2,5)}

Proof of correction:
A) If we have a vertex cover of size k in G, then i can find a collection of k sets whose union is equal to all of U. 
B) If I have a collection of k sets, whose union is equal to all of U, then I can find a vertex cover of size k in G. 

Given u bolean variable x1...xn a

a truth assignment fox X is an assignment of values 0 or 1 to each x_i

An assignment satisfies a change C if it causes C to evaluate 1.

An assignment satisfies a collection of if C1 and C2 and ... CK evaluate to 1.

Given a set of closures C1 to Ck over a set of variables x - {x1, ... xn} does there exist a satisfying truth 

Problem statement 
Given a set of 

Three set problem 
Given an 


C1 = (x1, x2, x3)



Efficient Certification

ex: indenpendent set problem.
certificate is a set of nodes of size at least k.
certifier: go over every edge and check the if they are not both in S the edge is OK.
check the size of set S.

Evaluate each clause 

If X \in NP and for all Y \in NP 


Is there a problem in NP that all other problem in NP can be reduced to it in polynomial time


Basic strategy to probe a problem X is NP-complete
1.prove X \in nP
2.choose a problem Y that is known to be NP-complete
3.Prove that Y≤pX