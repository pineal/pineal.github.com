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
- Tight bounds: \\(\Theta\\)-notation: \\(\Theta = \Omega \cap O\\)

Time complexities of some general algorithms can be ordered as:
$$ Ο(1)＜ Ο(\log{2n}) ＜ Ο(n) ＜ Ο(n\log2n) ＜ Ο(n^2) ＜ Ο(n^3) ＜ … ＜ Ο(2^n) ＜ Ο(n!) $$

Time complexity with //(Ο(\log{2n})//), //(Ο(n)//), //(Ο(n\log2n)//), //(Ο(n^2)//), //(Ο(n^3)//) are called polynomial time, and  //(Ο(2^n)//), //(Ο(n!)//) are exponential time. Problems concerning about polynomial time complexity are called P-problem and  the later one is called NP problems. Generally, algorithms with polynomial time complexity can be treated as efficient algorithm. 

###Examples
- Take the following list of functions and arrange them in ascending order of growth rate. That is, if function \\(g(n)\\) immediately follows function \\(f(n)\\) in your list, then it should be the case that $f(n)$ is \\( O(g(n))\\).

$$ f\_{1}(n) = n^{2.5} $$
$$ f\_{2}(n) = \sqrt{2n} $$
$$ f\_{3}(n)=n+10 $$
$$ f\_{4}(n)=10^{n} $$
$$ f\_{5}(n)=100^{n}$$
$$ f\_{6}(n)=n^{2}\log n $$

**Solution:** Growth rate can be arranged as ascending order like:
$$f\_{2}(n)<f\_{3}(n)<f\_{6}(n)<f\_{1}(n)<f\_{4}(n)<f\_{5}(n)$$

- Take the following list of functions and arrange them in ascending order of growth rate. That is, if function //(g(n)//) immediately follows function //(f(n)//) in your list, then it should be the case that //(f(n)/) is //(O(g(n))//).

$$ g\_{1}(n)=2^{\sqrt{\log n}}$$
$$ g\_{2}(n)=2^{n}$$
$$ g\_{4}(n)=n^{4/3}$$
$$ g\_{3}(n)=n(\log n)^{3}$$
$$ g\_{5}(n)=n^{\log n}$$
$$ g\_{6}(n)=2^{2^{n}}$$
$$ g\_{7}(n)=2^{n^{2}}$$

**Solution:** Growth rate can be arranged as ascending order like:
$$g\_{1}(n)<g\_{3}(n)<g\_{4}(n)<g\_{5}(n)<g\_{2}(n)<g\_{7}(n)<g\_{6}(n)$$
