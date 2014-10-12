---
layout: post
title: Divide and Conquer
categories:
- Algorithm
tags:
- Master Theorem
- Recursion
---

##Master Method
It is a cookbook method for solving the recurrence of the form:

$$T(n)= aT(n/b) + f(n)$$
where $a≥1$, $b≥1$ are constants, and $f(n)$ in an asymptotically positive function. a represents the number of subproblems and n/b represents size of the subproblem.

###Master Theorem 
Given the above definition of the recurrence relation, $T(n)$ can be bounded asymptotically as follows:
 
1. If $f(n)=O(n^{log_b^a - \epsilon})$ for some constant $\epsilon>0$ then $$T(b)= \theta(n^{log\_b^a})$$
2. If $f(n)=\theta(n^{log\_b^a}lg^kn)$, then $$T(n)=\theta(n^{log\_b^a}lg^{k+1}n)$$
3. If $f(n)=\Omega(n^{log\_b^a+\epsilon})$ for constant $\epsilon>0$ and if $af(n/b)≤cf(n)$ for some constant $c<1$ and all sufficiently large $n$, then $$T(n) = \theta(f(n))$$

**Examples**

1. $T(n)=5T（n/2） + \theta(n^2)$
2. $T(n)=27T（n/3） + \theta(n^3lgn)$
3. $T(n)=5T(n/2)+ \theta(n^3)$
4. $T(n)=27T（n/3） + \theta(n^3/lgn)$

**Solutions**

1. Since $n^{log\_2^5}>n^2$, use case 1 $\implies$ $T(n)=\theta(n^{lg5})$
2. Since $n^{log\_3^27} = n^3$, $f(n) = n^{log\_3^27 lgn}$ with $k=1$, case 2 $\implies$ $T(n)=\theta(log\_3^27 lg^2n)$
3. Since $n^{log\_2^5}<n^3$, use case 3 $\implies$ $T(n)=\theta(n^3)$
4. $f(n)=n^3lg^{-1}n$ with a constant $k=-1$, Master theorem can not be applied.

###Stock Problem
input: price of the stock on each day over a year period

output: where t_0 buy and where t_0 sell the stock to maximize profit

constraints: 
must buy before selling
must but all 1000shares in one day and sell all in one day

n-1 + n-2 +... +1
O(n^2)

**Solution:**
Case 1: buy and sell in P1, B=B1, S=S1, 
MN=min(MN1, MN2),MX=max(MN1,MN2)
Case 2: buy and sell in P2, B=B2, S=S2,
MN=min(MN1, MN2),MX=max(MN1,MN2)
Case 3: but in P1 and sell in P2, B=MN1, S=MX2,
MN=min(MN1, MN2),MX=max(MN1,MN2)

Divide takes: \theta(1)
Combine:\theta(1)
f(n)=\theta(1)
a=2, b=2

Complexity = \theta(n)

###Matrix Multiplication

matrice A \times B=C
Brutal force method takes \theta(n^3) 
$$\begin{matrix} a11 & a12 \\\\ a21 & a22 \end{matrix} \times \begin{matrix} b11 & b12 \\\\ b21 & b22 \end{matrix} = \begin{matrix} c11 & c12 \\\\ c21 & c22 \end{matrix}$$	

divide A, B, C to A11,A12,A21,A22
$$C11=A11\timesB11 + A12\timesB21$$
$$C12=A11\timesB12 + A22\timesB22$$
$$C21=A21\timesB11 + A22\timesB21$$
$$C22=A21\timesB12 + A22\timesB22$$

times to divide = O(n)
times to combine = O(n^2)
a=8, b=2
log_2^8=3
f(n)=O(n^2)
complexity=\theta(n^3)

###Strassen algorithm
P=(A11+A22)(B11+B22)
Q=(A21+A22)B11
R=A11(B12-B22)
S=A22(B21-B11)
T=(A11+A12)B22
U=(A21-A11)(B11+B12)
V=(A12-A22)(B21+B22)

C11=P+S-T+V
C12=R+T
C21=Q+S
C22=P+R-Q+V

f(n)=O(n^2)
log_2^7=2.81
complexity=\theta(n^2.81)
<http://en.wikipedia.org/wiki/Strassen_algorithm>

###Example
Input: unsorted array of size n
Output: Min and Max = the array
n-1 comparison to find min
n-1 comparison to find max
2n-2 comparison to find min and max
O(n)

Split two subarray recessively, 
last level: n comparison, last n/2 level 1 comparison
improvement -> 2n-2-n/2 = 3n/2 - 2

###Example
Find a closest set of points in a set of n points in a plot. 
Brute force: O(n^2)

p1 and p2
case 1: both pts are in p1
case 2: both pts are in p2
case 3: pt1 in p1, pt2 in p1

draw boundaries 


Implementation 

Choose  pair (P)
construct p_x: list of points sorted by x-coord
construct p_y: list of points sorted by y-coord

(p1,p2) = closest-pair-recursive(P_m,P_y)

if|P|<\epsilon
	solve it directly
else
	construct Qx left half of Px
	construct Qy list points of Qy
	construct Rx right half of Px
	construct Ry list points of Rx
	
(q0,q1) = closest-pair-Rec(Qx,Qy)
(r0,r1) = closest-pair-Rec(Rx,Ry)
\delta = min(d(q0,q1),d(r0,r1))	

S=points in P with distance \delta of L
Construct Sy ... set of pts in S sorted by y-coord
For each pt s in Sy, compute distance
	srom s to each of next 11 point in Sy let (s,s') be pair with min distance
	
if d(s,s') < \delta then
	return (s,s')
else if d(q0, q1) < d(r0, r1) then
	return (q0, q1)
else
		
	
<http://en.wikipedia.org/wiki/Closest_pair_of_points_problem>	
f(n)=O(n)
a=2, b=2
log_2ˆ2=1 
Case 2.

Complexity = O(n lgn)divide and conquer + O(n lgn)driving routine = O(n lgn) 


###Example
Find all nearest neighbors 
O(nlog n)