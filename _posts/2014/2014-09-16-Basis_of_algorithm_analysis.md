---
layout: post
title: Basis of algorithm analysis
categories:
- Algorithm
tags:
- time complexity
---



##Time Complexity
###Asymptotic Analysis
Look at growth of \\(T(n)\\) as \\(n\to\infty\\).

- Upper Bounds: \\(O\\)-notation 
- Lower Bounds: \\(\Omega\\)-notation
- Tight bounds: \\(\Theta\\)-notation
 -  \\(\Theta g(n) = \Omega g(n) \cap Og(n)\\)

Time complexities of some general algorithms can be ordered as:
$$ Ο(1)＜ Ο(\log{2n}) ＜ Ο(n) ＜ Ο(n\log2n) ＜ Ο(n^2) ＜ Ο(n^3) ＜ … ＜ Ο(2^n) ＜ Ο(n!) $$

Time complexity with \\(Ο(\log{2n})\\), \\(Ο(n)\\), \\(Ο(n\log2n)\\), \\(Ο(n^2)\\), \\(Ο(n^3)\\) are called polynomial time, and  \\(Ο(2^n)\\), \\(Ο(n!)\\) are exponential time. Problems concerning about polynomial time complexity are called P-problem and  the later one is called NP problems. Generally, algorithms with polynomial time complexity can be treated as efficient algorithm. 

###Exercises
**Q1.** Take the following list of functions and arrange them in ascending order of growth rate. That is, if function \\(g(n)\\) immediately follows function \\(f(n)\\) in your list, then it should be the case that \\(f(n)\\) is \\( O(g(n))\\).

$$ f\_{1}(n) = n^{2.5} $$
$$ f\_{2}(n) = \sqrt{2n} $$
$$ f\_{3}(n)=n+10 $$
$$ f\_{4}(n)=10^{n} $$
$$ f\_{5}(n)=100^{n}$$
$$ f\_{6}(n)=n^{2}\log n $$

**Solution:** Growth rate can be arranged as ascending order like:
$$f\_{2}(n)<f\_{3}(n)<f\_{6}(n)<f\_{1}(n)<f\_{4}(n)<f\_{5}(n)$$

**Q2.** Take the following list of functions and arrange them in ascending order of growth rate. That is, if function \\(g(n)\\) immediately follows function \\(f(n)\\) in your list, then it should be the case that \\(f(n)\\) is \\(O(g(n))\\).

$$ g\_{1}(n)=2^{\sqrt{\log n}}$$
$$ g\_{2}(n)=2^{n}$$
$$ g\_{3}(n)=n(\log n)^{3}$$
$$ g\_{4}(n)=n^{4/3}$$
$$ g\_{5}(n)=n^{\log n}$$
$$ g\_{6}(n)=2^{2^{n}}$$
$$ g\_{7}(n)=2^{n^{2}}$$

**Solution:** Growth rate can be arranged as ascending order like:
$$g\_{1}(n)<g\_{3}(n)<g\_{4}(n)<g\_{5}(n)<g\_{2}(n)<g\_{7}(n)<g\_{6}(n)$$

Obviously, we can certainly determine that g2 < g7 < g6 and they are exponential time. For g3 and g4, we divide them by n, it follows a log time and polynomial time, so g3 < g4.  for g5, the power is logarithmic, which is faster than a constant(g4). So g5 > g4. For g1, we replace the \\(\log n\\) with t, and do some operation to g3.By simply observation, a conclusion can be drew that g1 < g3.  

**Q3.** Assume you have functions f and g such that \\(f(n)\\) is \\(
O(g(n))\\). For each of the following statements, decide whether you think it is true or false and give a proof or counterexample.

- \\( \log{f(n)} \\) is \\( O(\log\_{2}g(n)) \\)
- \\( 2^{f(n)} \\) is \\( O(2^{g(n)}) \\)
- \\( f(n)^{2} \\) is \\( O(g(n)^{2}) \\)

**Solution:**

- **False**. Take the example of f(n) = 2 and g(n) = 1.
- **False**. Take the example of f(n) = 2n and g(n) - n.
- **True**. Refer to the definition of O-notation.


**Q4.** Consider the following basic problem. You’re given an array A consisting of n integers A[1], A[2], . . . , A[n]. You’d like to output a two-dimensional n-by-n array B in which BB[i, j], for i < j, contains the sum of array entries A[i] through A[j]—that is, the sum A[i] + A[i + 1] + . . . + A[j]. (The value of array entry B[i, j] is left unspecified whenever i ≥ j, so it doesn’t matter what is output for these values.) Here’s a simple algorithm to solve this problem.

```
for i=1, 2,...,n do	for j=i+1, i+2,...,n do		Add up array entries A[i] through A[j]		Store the result in B[i, j] 
	end forend for
```

- For some function f that you should choose, give a bound of the form O(f(n)) on the running time of this algorithm on an input of size n (i.e., a bound on the number of operations performed by the algorithm).

- For this same function f, show that the running time of the algorithm on an input of size n is also \\(\Omega(f(n))\\). (This shows an asymptotically tight bound of \\(\Theta(f (n))\\) on the running time.)

- Although the algorithm you analyzed in parts (a) and (b) is the most natural way to solve the problem—after all, it just iterates through the relevant entries of the array B, filling in a value for each—it contains some highly unnecessary sources of inefficiency. Give a different algorithm to solve this problem, with an asymptotically better running time. In other words, you should design an algorithm with running time \\(O(g(n))\\), where \\( \lim\
_{n\to\infty} g(n) /f (n) = 0 \\).

**Solution:**

- The outer loop of the given algorithm runs for exactly n iterations, and the inner loop of ht algorithm runs for at most n iterations. Therefore the line of code that adds up array entries A[i] trough A[j] is executed at most \\(n^2\\) times. Adding up any array entries A[i] through A[j] takes \\(O(j-i+1)\\), which is O(n). Store the result requires constant time. Therefore, the running time of the entire algorithm is at most \\(n^2O(n)\\), and so the algorithm runs in \\(O(n^3))\\.
- Remain to be solved.

- Look at the algorithm listed below:

```
for i =1, 2, ..., n do 
	Set B[i, i+1] to A[i] + A[i+1]
end for
for k = 2, 3, ..., n-1 do
	for i = 1,2,...n-k do 
		j = i + k
	B[i,j] to be B[i, j-1] + A[j]
	end for
end for
```





 
**Q5.** Suppose we have an array A of n integers and wish to compute an n x n array B where B[i,j] holds the sum of A[i] through A[j]. The following code fragment is one way to solve this problem.

 - What is the running time of the code fragment? (Hint: it is \\(Ω(n^2)\\) but not \\( O(n^2) \\)
 - Do you suppose this can be solved in time better than \\(O(n^2)\\)? Why or why not?
 - Give a \\(O(n^2)\\) time solution to this problem.
 
 **Solution:**
 
 
