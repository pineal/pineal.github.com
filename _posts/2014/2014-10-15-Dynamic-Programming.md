---
layout: post
title: Dynamic programming
categories:
- Algorithm
tags:
- dynamic programming
---

General approach to solving optimal problems, using dynamic programming

1. Characterize the structure of an optimal solution.
2. Recursively define the value of an optimal solution.
3. Compute the value of an optimal solution in a bottom up fashion.
4. Construct an optimal solution from computed from Step 3.

Problem Def.
We have 1 resource, n request labeled from 1 to n, each request having start time $s_i$ and finish time $f_i$ and weight $w_i$.
Goal: select a subset S $\subseteq$ {1,...n} of mutually compatible intervals so as to maximize sum of weight. 

Observation:

- Either job i is part of the optimal solution or not

 
**Case 1**
If it is, value of the optimal solution = w_i + value of the optimal solution for the subproblem that consists only of compatible request with i.

**Case 2** 
If it isn't. value of optimal solution = value of optimal solution without job i.

Explore both paths recursively and find out which is optimal

Value of optimal solution = $max$[value for case 1, value for case 2]

Sort request in order of non-decreasing finish time

$f_1≤f_2≤...f_n$

Define p(j) for an interval j to be the largest index i<j such that interval i & j are disjoint

Def Let Oj denote the optimal solution to the problem consisting of requests {i...j}

Let OPT(j) denote the value of Oj

case 1 j \in Oj \implies OPT(j) = w_j + OPT(p(j))

case 2 j \notin Oj \implies OPT(j) = OPT(j-1)

solution: 

```	
	compute-opt(j)
		if j =0 then 
			return 0
		else 
			return max(w_j + compute-opt(p_j), compute-opt(j-1))
		end if
```
This algorithm Run exponatially	


T(n) = T(n-1) + T(n-2) + C

Memorization
- Store the value of compute-opt in a globally accessible place.The print time are computed it, then simplify are this precomputed value in place of all future recursive calls.

M compute opt(j)
if j=0 then 
	return 0
else if M[j] is not empty then 
	return M[j]
else 
	define M[j] = max(w_j + M-compute-opt(p(j)), M-compute-opt(j-1))
return M[j]
endif

Take $\mathcal{O}(n)$

Sorting O(nlgn)
comstruct p(j) O(nlgn)
time spend in M-compute-opt O(n) 


computing an opt solution 

j belongs to Oj iff 

```
Find-solution
	if j>0 then 
		if w_j + M[P(j)] ≥ M[j-1] then 
		 output j together with the result of Find-solution(P(j))
		else
		output the result of
			Find-solution(j-1)
		Endif	
	Endif
```

```
M[0] = 0
for i = 1 for 
	M[j] = max(w_j+M[p(j)], M[j-1])
endfor
```

\mathcal{O}(n)






opt(i) denotes the opt energy level at step i

Case 1 you took one step
case 2 you jump over one square
case 3 you jump over two squares

OPT(n)=max(OPT(n-1) - 50 - e_n, OPT(n-2) - 150 -e_n, OPT(n-3) - 350 - e_n)

OPT(0)=0, OPT(1)=-50-e_1
```
for(i = 1 to n)
	OPT(n)=max(OPT(n-1) - 50 - e_n, OPT(n-2) - 150 -e_n, OPT(n-3) - 350 - e_n)
endfor
```
0-1 knapsack & subsetsum
- A single machine 
- requests {1...n} each take time w_i to process
- can schedule jobs at any time between 0 to w
- objective to schedule jobs such that we maximize the machine's utilization

OPT(i) = value of the opt solution for requests 1 to i
if n not \in O then OPT(n) = OPT(n-1)
if n \in O then OPT(n) = w_n + OPT(n-1)

OPT(i, w) = value of the opt solution using a subset of this item {1...i} with maximum allowed weight w. 
if n not in O then OPT(n,w) = OPT(n-1, w)
if n \in O then OPT(n,w) = w_n + OPT(n1, w-w_n)
if w<w_i then OPT(i,w) = OPT(i-1, w)

otherwise OPT(i,w) = Max(OPT(i-1,w), w_i + OPT(i-1, w-w_i))


subset-sum(n,w)
array M[0, w] = 0 for each w=0 to w

for i = 1 to n
	for w = 0 to W
		use recurrence formula 1. to compute M[i,w]
	endfor
endfor
return M[n, w]
take O(nw)
pseudo-polynominal  

let OPT(i) denote the opt. no of coins to make exact change for i scheduling.

OPT(n) = min(OPT(n+25)+1,OPT(n-20)+1,OPT(n-10)+1,OPT(n-5)+1,OPT(n-1)+1)


for i = 25 to n
	we recurrence formula
endfor