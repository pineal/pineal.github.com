---
layout: post
title: Two Sequences Dynamic Programming
categories:
- Algorithm
tags:
- Dynamic Programming
date : "2015-10-10T00:00:00"
---
## Questions
两个序列类型的动态规划。

- [Longest Common Sequences](http://www.lintcode.com/en/problem/longest-common-subsequence/)
- [Edit Distance](http://www.lintcode.com/en/problem/edit-distance/)
- [Interleaving String](http://www.lintcode.com/en/problem/interleaving-string/)
- [Distinct Subsequences](http://www.lintcode.com/en/problem/distinct-subsequences/)

## Longest Common Sequences
LCS是的经典动归问题。

~~~cpp
class Solution {
public:
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    int longestCommonSubsequence(string A, string B) {
        // write your code here
        int dp[A.size() + 1][B.size() + 1];

        for (int i = 0; i < A.size() + 1; ++i){
            dp[i][0] = 0;
        }

        for (int i = 0; i < B.size() + 1; ++i){
            dp[0][i] = 0;
        }

        for (int i = 1; i < A.size() + 1; ++i){
            for (int j = 1; j < B.size() + 1; ++j){
                if (A[i - 1] == B[j - 1]){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else{
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
    return dp[A.size()][B.size()];    
    }
};
~~~

## Edit Distance

设状态为dp[i][j]，表示T[0, j]在S[0, i]里出现的次数。


状态转移方程可以用一下公式来表达：

1. 如果 $$word1[i] == word2[j]$$，

    $$dp[i][j] = dp[i - 1][j - 1]$$


2. 如果 $word1[i] != word2[j]$,

    $$
      \begin{aligned}
     dp[i][j] & =  min ( \\
       & = dp[i - 1][j] + 1,(在word1中删除一个字符word1[i])   \\
       & = dp[i][j - 1] + 1,（在word1中插入一个字符word2[j]）   \\
       & = dp[i - 1][j - 1] + 1（替换word1[i] == word2[j]）    \\
       & )    \\
      \end{aligned}
    $$


[参考](http://yutianx.info/2014/09/26/2014-09-26-leetcode-edit-distance/)

~~~cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        const int len1 = word1.size();
        const int len2 = word2.size();

        int dp[len1 + 1][len2 + 1];
        for (int i = 0; i <= len1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= len2; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1]));  
                }
            }
        }
        return dp[len1][len2];
}
};
~~~

##  Interleaving String
~~~cpp
class Solution {
public:
    /**
     * Determine whether s3 is formed by interleaving of s1 and s2.
     * @param s1, s2, s3: As description.
     * @return: true of false.
     */
    bool isInterleave(string s1, string s2, string s3) {
        // write your code here
        int len1 = s1.size(), len2 = s2.size(), len3 = s3.size();
        if (len1 + len2 != len3) return false;
        //代表s1前i个字符和s2前j个字符能否组成s3前i+j个字符

        bool dp[len1 + 1][len2 + 1];
        dp[0][0] = true;
        //初始化：s1前i个字符和s2前0个字符能否组成s3前i个字符？
        //只要能一一配对，即为true 否则为 false
        for (int i = 1; i < len1 + 1; ++i){
            dp[i][0] = (s3[i-1] == s1[i-1])? true : false;
        }

        for (int j = 1; j < len2 + 1; ++j){
            dp[0][j] = (s3[j-1] == s2[j-1])? true : false;
        }
        // 比较 s3 第i + j 个字符 和 s1第i个字符 或者 s2第j个字符
        for (int i = 1; i < len1 + 1; ++i){
            for (int j = 1; j < len2 + 1; ++j){
                if ((s3[i + j - 1] == s1[i - 1] && dp[i - 1][j]) || (s3[i + j - 1] == s2[j - 1] && dp[i][j - 1])){
                    dp[i][j] = true;
                }
                else{
                    dp[i][j] = false;
                }
            }
        }
        return dp[len1][len2];   
    }
};
~~~


## Distinct Subsequences
~~~cpp
class Solution {
public:
    /**
     * @param S, T: Two string.
     * @return: Count the number of distinct subsequences
     */
    int numDistinct(string &S, string &T) {

        if (S.size() < T.size()) return 0;
        if (T.empty()) return 1;

        vector<vector<int> > dp(S.size() + 1, vector<int>(T.size() + 1, 0));

        int lenS = S.size();
        int lenT = T.size();

        for (int i = 0; i < S.size(); ++i) {
            dp[i][0] = 1;
            for (int j = 0; j < T.size(); ++j) {
                if (S[i] == T[j]) {
                    dp[i + 1][j + 1] = dp[i][j + 1] + dp[i][j];
                } else {
                    dp[i + 1][j + 1] = dp[i][j + 1];
                }
            }
        }

        return dp[S.size()][T.size()];
    }
};
~~~
