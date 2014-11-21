---
layout: post
title: Networking Flow
categories:
- Algorithm
tags:
- 
---



Def A flow network is a directed graph G=(V,E) with following features:

- each edge e has a non-negative capacity c_e
- has a single source node s \in V
- has a single sink node T

Assumptions - no edges enter s and leave t

-at least one edge connected to each node

-all capacities are integers

Notation: we call f(e) flow over edge e.

 f(e) has following properties
 
 1. For each node e $\in$ E 0≤f(e)≤c_e
 2. for each node v other than s&t
  
 \sigma_{e in of V}f(e) =  \sigma_{e out of V}f(e)
 
Flow is steady state

Def for a steady state flow, the value of a flow is 
	V(f) = \sigma_{e out of V}f(e)
	
can also say 	

f^{out}(s) = \sigma_{e out of V}f(e)

f^{in}(s) = \sigma_{e in of V}f(e)



Def Gf in the graph of G with the following definition

-	G_f has the same set of nodes as G
-	For each edge e with f(e)<c_e we include e in G_f with caprice c_e - f(e) (forward edge)
-	For each edge e with f(e)>0 we include edge e' (opposite direction foe)














