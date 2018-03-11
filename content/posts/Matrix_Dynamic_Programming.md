---
layout: post
title: Matrix Dynamic Programming
categories:
- Algorithm
tags:
- Dynamic Programming
date : "2015-09-30T00:00:00"
---
## Questions
- [Unique Paths](http://www.lintcode.com/en/problem/unique-paths/)
- [Min Path Sum](http://www.lintcode.com/en/problem/minimum-path-sum/)

一般这类问题都是给了一个m＊n的矩阵(或者三角形)，从一个方向走到另一个方向（从顶到底，或者从左上角到右下角 .etc），一般来说只会朝固定的方向走。

初始化两条边，然后根据条件循环更新状态方程，主要看上一步的状态，最后得出结果。

## Unique Paths

~~~cpp
class Solution {
public:
    /**
     * @param n, m: positive integer (1 <= n ,m <= 100)
     * @return an integer
     */
    int uniquePaths(int m, int n) {
        // wirte your code here

    int dp[m][n];

        for (int i = 0; i < m; i++){
            dp[i][0] =  1;
        }

        for (int i = 0; i < n; i++){
            dp[0][i] =  1;
        }

        for (int i = 1;i < m; i++){
            for (int j = 1; j < n;j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }
};
~~~



## Min Path Sum


~~~cpp
class Solution {
public:
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    int minPathSum(vector<vector<int> > &grid) {
        // write your code here
        int m = grid.size(), n = grid[0].size();
        int sum[m][n];

        sum[0][0] = grid[0][0];

        for (int i = 1; i < m; i++){
            sum[i][0] =  sum[i-1][0] + grid[i][0];
        }

        for (int i = 1; i < n; i++){
            sum[0][i] =  sum[0][i-1] + grid[0][i];
        }

        for (int i = 1; i < m; i++){
            for (int j = 1 ; j < n; j++){
                sum[i][j] = min(sum[i-1][j], sum[i][j-1]) + grid[i][j];
            }
        }
        return sum[m-1][n-1];
    }
};
~~~
