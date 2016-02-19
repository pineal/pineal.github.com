---
layout: post
title: Intro to Dynamic Programming
categories:
- Algorithm
tags:
- Dynamic Programming
---


## Concepts of Dynamic programming
General approach to solving optimal problems, using dynamic programming

1. Characterize the structure of an optimal solution.
2. Recursively define the value of an optimal solution.
3. Compute the value of an optimal solution in a bottom up fashion.
4. Construct an optimal solution from computed from Step 3.

## Weighted Interval Scheduling

### Problem Definition and analyzing
We have 1 resource, n request labeled from 1 to n, each request having start time $s_i$ and finish time $f_i$ and weight $w_i$.

Goal:

- select a subset S $\subseteq$ {1,...n} of mutually compatible intervals so as to maximize sum of weight.

Observation:

- Either job i is part of the optimal solution or not


**Case 1**

If it is, value of the optimal solution = $w_i$ + value of the optimal solution for the subproblem that consists only of compatible request with i.

**Case 2**

If it isn't. value of optimal solution = value of optimal solution without job i.

Explore both paths recursively and find out which is optimal

Value of optimal solution = $max$[value for case 1, value for case 2]

Sort request in order of non-decreasing finish time: $f_1≤f_2≤...f_n$

Define $P(j)$ for an interval j to be the largest index i < j such that interval i & j are disjoint

Def Let $\mathcal{O}(j)$ denote the optimal solution to the problem consisting of requests {i...j}

Let $OPT(j)$ denote the value of $\mathcal{O}(j)$

**case 1** $j \in \mathcal{O}(j)$ $\implies$ $OPT(j) = w_j + OPT(p(j))$

**case 2** $j \notin \mathcal{O}(j)$ $\implies$ $OPT(j) = OPT(j-1)$


### Naive Solution


~~~
compute-opt(j)
	if j =0 then
		return 0
	else
		return max(w_j + compute-opt(p_j), compute-opt(j-1))
	end if
~~~

This algorithm Run exponentially:

$$T(n) = T(n-1) + T(n-2) + C$$

### Memorization

- Store the value of compute-opt in a globally accessible place.The print time are computed it, then simplify are this precomputed value in place of all future recursive calls.


~~~
M-compute-opt(j)
if j=0 then
	return 0
else if M[j] is not empty then
	return M[j]
else
	define M[j] = max(w_j + M-compute-opt(p(j)), M-compute-opt(j-1))
return M[j]
endif
~~~

This algorithm takes $\mathcal{O}(n)$
In total we have:
- Sorting $\mathcal{O}(n log n)$
- construct p(j) $\mathcal{O}(n log n)$
-time spend in M-compute-opt $\mathcal{O}(n)$
Therefore, computing an opt solution takes $\mathcal{O}(n)$.

### Find-Solution
~~~
Find-solution(j)
	if j>0 then
		if w_j + M[P(j)] ≥ M[j-1] then
		 output j together with the result of Find-solution(P(j))
		else
		output the result of
			Find-solution(j-1)
		Endif
	Endif
~~~

~~~
M[0] = 0
for i = 1 to n
	M[j] = max(w_j+M[p(j)], M[j-1])
endfor
~~~

This algorithm takes $\mathcal{O}(n)$.

___
opt(i) denotes the opt energy level at step i

Case 1 you took one step
case 2 you jump over one square
case 3 you jump over two squares

OPT(n)=max(OPT(n-1) - 50 - e_n, OPT(n-2) - 150 -e_n, OPT(n-3) - 350 - e_n)

OPT(0)=0, OPT(1)=-50-e_1

~~~
for(i = 1 to n)
	OPT(n)=max(OPT(n-1) - 50 - e_n, OPT(n-2) - 150 -e_n, OPT(n-3) - 350 - e_n)
endfor
~~~
___


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


Discussion

- Recursive
- Optimal
- all subproblems are the same

Problem
Graduate students get a lot of free food at various events. Suppose you have a schedule of the next n days marked with those days when you get a free dinner, and those days on which you must  acquire dinner on your own. On any given day you can buy dinner at the cafeteria for 3 dollars. Alternatively, you can purchase one week's groceries for 10 dollars, which will provide dinner for each day that week. However, because you don't have a fridge, the groceries will go bad after seven days and any leftovers must be discarded. Due to your very busy schedule, these are your only two options for dinner each night. Your goal is to eat dinner every night while minimizing the money you spend on food.

Solution 1
define OP(n) to be the cheapest way to eat for day 1..n
OP(n) = OP(n-1) if day n has free food
If eating at the cafeteria, OP(n) = OP(n-1) + 3
If eating groceries, OP(n) = OP(n-6) + 10
If day n does not have free food, OP(n) = min(OP(n-1) + 3, OP(n-6) + 10)
Assume OP(n≤0) = 0


```
i = n
	while i >0
		if free food on day i
			output
```

When their respective sport is not in season, USC's student-athletes are very involved in their community, helping people and spreading goodwill for the school. Unfortunately, assume each student-athlete is limited to at most one community service project per semester, so the athletic department is not always able to help every deserving charity. For the upcoming semester, we have s student-athletes who want to volunteer their time, and B buses to help get them between campus and the location of their volunteering. Each bus can only


## Sequence Alignment


A DNA strand consists of a string of molecules called bases

A, C, G, T

Line them up to find similarity

e.g.

S1 = A|CC|_ |GGT|C|G|_

S2 = _|CC|A |GGT|G|G|C

Three gaps and one mismatch

**Problem**
Suppose we have 2 string X&Y

X={x_1,x_2,...x_m}
Y={y_1,y_2,...y_m}

Def: a matching in a set of ordered pair with property that each item occurs at most once.

Caring
Racing

Def: a matching is an alignment if there are no crossing pairs.

(i,j)< (i',j') $\in$ M & i<i' $\implies$ must have j<j'

For a given alignment M between X&Y

- We incur a gap penalty or cost of $\delta$
- For each mismatch of letters p and q we incur a mismatch cost $\alpha_{pq}$


Similarity between string X and Y is the minimum cost in alignment between X and Y.  

Observation: either (x_m, y_n) $\in$ M or not
Three base cases.



Define OPT(i,j) on the main cost of an alignment between x1, .. xi & y1, .. yi

Case 1.
(x_m, y_n) \in M \implies OPT(m,n) - OPT(m-1,n-1)+\alpha x_m y_n

Case 2.
x_m is not matched
OPT(m,n) = OPT(m-1, n) + \delta

Case 3.
Yn is not matched
OPT(m,n) = OPT(m,n-1) + \delta


Recursion
OPT(i,j) = min[\alpha x_iy_j + OPT(i-1,j-1), \delta + OPT(i-1, j), \delta + OPT(i-1, j)]

```
Alignment (x,y)
Initialize A[i,0] = i\delta for each i
			   A[0,j] = j\delta for each j  
for j = 1 to n
	for i = 1 to m
		A[i,j] = min[\alpha x_iy_j + A(i-1,j-1), \delta + A(i-1, j), \delta + A(i-1, j)]
	endfor
endfor
return A[m,n]
```

complexity = $\mathcal{O}(mn)$

X |_____X_L_____|_____X_R_____|
Y |________________________________|

X -> Divide and conquer
Y -> Dynamic programming to find a break point


## Shortest path problem
Bellman-Ford algorithm
If G has no negative cycles. Then there is a shortest path from S to T that is simple and hence has at most n-1 edges.

OPT(i,v) denotes the minimum cost of a v-t path using at most i edges.

We want to compute OPT(n-1, s)

Find the optimal
Let P be an opt path
either P uses at most i-1 edges $\implies$ OPT(i,v) = OPT(i-1,v)
or if P uses i edges and the first edge in (v,w) the OPT(i,v) = C_{vw} + OPT{i-1, w}
w \in Adj(v)

OPT(i,v) = min(OPT(i-1,v), min(OPT(i-1, w)) + c_{vw})


```
Shortest path (G, s, t)
	n = no of node in G
	define M[0, t] = 0
		M[0,v] = \infinity
	for i = 1 to n - 1
		for v \in V in any order
		M[i,v] = min{M[i-1, v], min{M[i-1, w]+C_{vw}}} w \in adj(v)
		endfor
	endfor
return M[n-1,s]
```


$\mathcal{O}(n^3)$
	Complexity of Belllman Ford $\mathcal{O}(mn)$ ???why


Brute force will cost
better approach from BF add a new node s
	cost will be $\mathcal{O}(mn)$

BellaFord  vs  Dijkstra's
O(mn)  	vs	 O(m log n)
