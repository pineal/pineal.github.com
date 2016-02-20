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
- [Longest Increasing Subsequence](http://www.lintcode.com/en/problem/longest-increasing-subsequence/)
- [Word Break](http://www.lintcode.com/en/problem/word-break/)

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


## Longest Increasing Subsequence

~~~cpp
class Solution {
public:
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    int longestIncreasingSubsequence(vector<int> nums) {
        // write your code here
        if (nums.empty()) return 0;

        vector<int> dp(nums.size(), 1);
        int rst = 0;

        for (int i = 1; i < nums.size(); i++){
            for(int j = i - 1; j >= 0; j--){
                if (nums[j] <= nums[i]){
                    dp[i] = max(dp[i], dp[j] + 1);
                }                             
            }
            rst = max(rst, dp[i]);
        }
        return rst;
    }
};

~~~


## Word Break

在做了分割字符串II那道题之后，发现其实这两题一样一样的。

1. State: f[i] 表示前i个字符能否根据词典中的词被成功分词。

2. Function: f[i] = or{f[j], j < i, letter in [j+1, i] can be found in dict}, 含义为小于i的索引j中只要有一个f[j]为真且j+1到i中组成的字符能在词典中找到时，f[i]即为真，否则为假。具体实现可分为自顶向下或者自底向上。

3. Initialization: f[0] = true, 数组长度为字符串长度 + 1，便于处理。

4. Answer: f[s.length]


~~~cpp
class Solution {
public:
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    bool wordBreak(string s, unordered_set<string> &dict) {
        // write your code here
        if (s.empty()) return true;
        if (dict.empty()) return false;

        int len = s.size();
        int max_word_len = 0;

        for (unordered_set<string>::iterator it = dict.begin(); it != dict.end(); ++it) {
            int thisLen = (*it).size();
            max_word_len = max(max_word_len, thisLen);
        }

        deque<bool> canBreak(len + 1, false);
        canBreak[0] = true;

        for (int i = 1; i < len + 1; ++i){
            for (int j = i - 1; j >= 0; --j){

                if (i - j > max_word_len) break;


                if (canBreak[j] && dict.find(s.substr(j, i - j))!= dict.end()){
                    canBreak[i] = true;
                    break;
                }   
            }
        }
    return canBreak[len];    
    }
};


~~~
