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

## # ### M compute opt(j)
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



		 	
		