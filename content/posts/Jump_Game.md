---
layout: post
title: Jump Game
categories:
- Algorithm
tags:
- Greedy
- Dynamic Programming
date : "2016-02-19T00:00:00"
---

## Questions

* [Jump Game](http://www.lintcode.com/en/problem/jump-game/)
* [Jume Game II](http://www.lintcode.com/en/problem/jump-game-ii/)


这两题明眼看是序列形动态规划的题，但是时间复杂度上其实并不好，会出现超时的情况。而用贪心法则能避免。单独拿出来记录一下贪心法。尤其是Jump Game II 的做法非常巧妙。

## Jump Game

动态规划解法，无法被AC, c++的解法会超时。

~~~cpp
class Solution {
public:
    /**
     * @param A: A list of integers
     * @return: The boolean answer
     */
    bool canJump(vector<int> A) {
        if (A.empty()) {
            return true;
        }

        vector<bool> jumpto(A.size(), false);
        jumpto[0] = true;

        for (int i = 1; i != A.size(); ++i) {
            for (int j = i - 1; j >= 0; --j) {
                if (jumpto[j] && (j + A[j] >= i)) {
                    jumpto[i] = true;
                    break;
                }
            }
        }

        return jumpto[A.size() - 1];
    }
};
~~~

贪心法的解法，很快。

维护一个最远能到达的变量，如果当前位置的值超过了这个变量那么更新该变量。从头循环到尾，如果现在的位置超过了最远能到达的位置，那说明到不了最后，返回false。

~~~cpp
class Solution {
public:
    /**
     * @param A: A list of integers
     * @return: The boolean answer
     */
    bool canJump(vector<int> A) {
        // write you code here

        int reachable = 0;
        for (int i = 0; i < A.size(); ++i){
            if (i > reachable){
                return false;
            }
            reachable = max(reachable, i + A[i]);
        }
        return true;
    }
};
~~~


## Jump Game II

动态规划的和之前的没什么大差别，把维护的数组类型从bool换成了int记录步数。

~~~cpp
class Solution {
public:
    /**
     * @param A: A list of lists of integers
     * @return: An integer
     */
    int jump(vector<int> A) {
        if (A.empty()) {
            return -1;
        }

        const int N = A.size() - 1;
        vector<int> steps(N, INT_MAX);
        steps[0] = 0;

        for (int i = 1; i != N + 1; ++i) {
            for (int j = 0; j != i; ++j) {
                if ((steps[j] != INT_MAX) && (j + A[j] >= i)) {
                    steps[i] = steps[j] + 1;
                    break;
                }
            }
        }

        return steps[N];
    }
};
~~~


贪心法：

~~~cpp
// Time:  O(n)
// Space: O(1)

class Solution {
public:
    /**
     * @param A: A list of lists of integers
     * @return: An integer
     */
    int jump(vector<int> A) {
        int jump_count = 0;
        int reachable = 0;
        int curr_reachable = 0;
        for (int i = 0; i < A.size(); ++i) {
            if (i > curr_reachable) {
                // current jumps are not enough,
                // jump one more step, which enlarges curr_reachable to reachable.
                curr_reachable = reachable;
                ++jump_count;
            }
            reachable = max(reachable, i + A[i]);
        }   

        return jump_count;
    }
};

~~~


[reference](http://www.kancloud.cn/kancloud/data-structure-and-algorithm-notes/73079)
