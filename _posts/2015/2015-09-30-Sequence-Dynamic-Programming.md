---
layout: post
title: Sequence Dynamic Programming
categories:
- Algorithm
tags:
- Dynamic Programming
---
## Questions
- [Climbing Stairs](http://www.lintcode.com/en/problem/unique-paths/)
- [House Robber](http://www.lintcode.com/en/problem/house-robber/)

## Climbing Stairs

状态转移方程，两种可能：要么是从i-1爬到i的，要么就是从i-2爬到i的，要计算所有可能的爬法之要把两者相加即可。所以维护一个数组来记录到i的爬法总数，那么:

$$
f[i] = f[i-1] + f[i-2]
$$

~~~cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: An integer
     */
    int climbStairs(int n) {
        // write your code here
        if (n <= 1){
            return 1;
        }

        vector<int> opt = {0, 1, 2};

        for (int i = 3; i <= n; i++){
            opt.emplace_back(opt[i-1] + opt[i-2]);
        }

        return opt[n];
    }
};
~~~


## House Robber

这道题很像学校里上算法课的时候给的例题。最重要的是想清楚状态转移方程是怎么样的。常规的动归思路：维护一个相同大小的一维数组，因为是一个累积的过程，并且不能取邻近位置，那么在位置i的最优解只有两种可能：不取位置i上的值，那么这个最优值就跟i-1位置上的一样；或者取i上的值加上i-2上的最优值。比较这两者之间的大小即可。这类问题可以枚举初始状态，然后顺着这个思路写出状态转移方程。并且一般都是从头到尾的一次循环。

$$
f[i] = max(f[i-1], f[i-2] + A[i])
$$


~~~cpp
class Solution {
public:
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
long long houseRobber(vector<int>& nums) {
    int len = nums.size();
    if (len == 0) return 0;
    if (len == 1) return nums[0];

    vector<long long> s = {nums[0], max(nums[0],nums[1])};

    for (int i=2;i<len;i++){
        s.emplace_back(max(s[i-1], s[i-2] + nums[i]));
    }

    return s[len-1];
    }

};
~~~
