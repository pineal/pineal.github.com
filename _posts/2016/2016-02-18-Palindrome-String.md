---
layout: post
title: Palindrome Partitioning
categories:
- Algorithm
tags:
- Search
- DFS
- Backtracking
- Dynamic Programming
---

## Questions

分隔回文串问题，共有两题，分别是搜索和动归的代表题型。刚碰到的时候理解比较难，单独拿出来看一看。

* [Palindrome Partitioning](http://www.lintcode.com/en/problem/palindrome-partitioning/)
* [Palindrome Partitioning II](http://www.lintcode.com/en/problem/palindrome-partitioning-ii/)

## Palindrome Partitioning II
求最小的分割次数，一维的一个数组，满足动态规划的条件。
提示：动归字符串时基本上要把f[0]空出来，这样就需要n+1长度的一个数组来记录最优值。

~~~cpp
class Solution {
public:
    int minCut(string s) {
        int len = s.size();
        if (len <= 1) return 0;

//用一个矩阵来记录子字符串s[i:j]是否为回文串

        vector<vector<bool> > mat = vector<vector<bool> >(len, vector<bool>(len, true));

        for (int i = len; i >= 0; --i) {
            for (int j = i; j < len; ++j) {
//                if((i+1>j-1 || isPal[i+1][j-1]) && s[i]==s[j])
//                    isPal[i][j] = true;                
//很多答案给的是这样的判断方法，个人觉得没有下面的清楚，边界条件实际上就两种，要么i==j要么i和j靠在一起。其他就判断xSx是不是回文串（如果S是的话）              
                if (j == i) {
                    mat[i][j] = true;
                } else if (j == i + 1) {
                    mat[i][j] = (s[i] == s[j]);
                } else {
                    mat[i][j] = ((s[i] == s[j]) && mat[i + 1][j - 1]);
                }
            }
        }

        vector<int> cut(len + 1, INT_MAX);

// 真正的sequence DP: cut[i]表示到i的minCut
// 到位置i时候，就判断j+1到i是不是一个回文串（到角标就变成了和j , i-1）
// 就找所有比i小的j的位置上能切的minCut + 1（此时条件为上面那个矩阵）
//

        for (int i = 1; i < 1 + len; ++i) {
            for (int j = 0; j < i; ++j) {
                if (mat[j][i - 1]) {
                    cut[i] = min(cut[i], 1 + cut[j]);
                }
            }
        }

        return cut[len];
    }
};
~~~
还有一个[优化的解](https://leetcode.com/discuss/9476/solution-does-not-need-table-palindrome-right-uses-only-space)以后留着看。


## Palindrome Partitioning
这题求的是具体的分隔方法，基本上就是DFS搜索加回溯的方法。

~~~cpp
class Solution {
public:
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    vector<vector<string>> partition(string s) {
        vector<vector<string> > result;
        if (s.empty()) return result;

        vector<string> palindromes;
        dfs(s, 0, palindromes, result);

        return result;
    }

private:
    void dfs(string s, int pos, vector<string> &palindromes,
             vector<vector<string> > &ret) {

        if (pos == s.size()) {
            ret.push_back(palindromes);
            return;
        }

        for (int i = pos + 1; i <= s.size(); ++i) {
            string substr = s.substr(pos, i - pos);
            if (!isPalindrome(substr)) {
                continue;
            }

            palindromes.push_back(substr);
            dfs(s, i, palindromes, ret);
            palindromes.pop_back();
        }
    }

    bool isPalindrome(string s) {
        if (s.empty()) return false;

        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] != s[n - i - 1]) return false;
        }

        return true;
    }
};
~~~
