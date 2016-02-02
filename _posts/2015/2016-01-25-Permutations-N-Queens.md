---
layout: post
title: Permutations and N-Queens
categories:
- Algorithm
tags:
- search
- DFS
- backtracking
- Permutations
- N-Queens
---
# Permutations and N-Queens

##Permutation from STL Library
最直接的做法：C++11中的STL中有关于排列的algorithm库可以直接用。

- [std::is_permutation](http://en.cppreference.com/w/cpp/algorithm/is_permutation)
- [std::next_permutation](http://en.cppreference.com/w/cpp/algorithm/next_permutation)
- [std::prev_permutation](http://en.cppreference.com/w/cpp/algorithm/prev_permutation)

- Changes the order of the elements in [Begin, end) acccording to the next permutation.
- Return *False* if the elements got the "normal"(lexicographical) order: that is, ascending order. So, to run through all permutations, you have to sort all elements and start a loop that calls this function as long as these algorithms return true.


```c++
class Solution:
public:
    vector<vector<int>> permute(vector<int> nums){
    vector<vector<int>> result;

    if (nums.empty()){return {}};
    std::sort(nums.begin(), nums.end());

    do{
        result.emplace_back(nums);
    } while(next_permutation(nums.begin(), nums.end()));
    return result;
    }
};    
```

###Implementation of std::next_permutation()
大致思路为，我们先固定第一个数，然后对右边剩下的数做全排列。什么时候右边剩下的数完成了全排列呢？那就是当这些数变成了降序。然后我们才用第一个数。

[留着慢慢消化，反正直接让我写，我是写不出来。](http://stackoverflow.com/questions/11483060/stdnext-permutation-implementation-explanation)
```c++
//kan bu dong...
template<class BidirIt>
bool next_permutation(BidirIt first, BidirIt last)
{
    if (first == last) return false;
    BidirIt i = last;
    if (first == --i) return false;

    while (true) {
        BidirIt i1, i2;

        i1 = i;
        //if the elements are in ascending order
        if (*--i < *i1) {
            i2 = last;
            //find the next largest digit
            while (!(*i < *--i2));
            // and put it in front
            std::iter_swap(i, i2);
            //put the remaining digits in ascending order
            std::reverse(i1, last);
            return true;
        }
        //last purmutation
        if (i == first) {
            std::reverse(first, last);
            return false;
        }
    }
}

```
有了这个之后我们可以自己模拟 std::next_permutation()

##Recursion Version for Permutations
常规的backtracking回溯，用DFS递归就行。
```c++
class Solution {
public:
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    vector<vector<int> > permute(vector<int> nums) {
        // write your code here
        vector<vector<int>> rst;
        if (nums.size() == 0) return rst;
        vector<int> v;
        backtracking(rst, nums, v);
        return rst;
    }


    void backtracking(vector<vector<int>>& rst, vector<int>& nums, vector<int>& v){
        if (v.size() == nums.size()){
            rst.emplace_back(v);
            return ;
        }

        for (int i = 0; i < nums.size(); ++i){
            //can employ map to improve the time complexity
            //vector<bool> is not a container. do not use. alternates: deque<bool>, or bitset
            if (find(v.begin(), v.end(), nums[i]) != v.end()){
                continue;
            }
            v.emplace_back(nums[i]);
            backtracking(rst, nums, v);
            v.pop_back();
        }
    }
};

```
##Permutations II
全排列去重。在循环的过程中加入while语句跳过相同的元素。
```c++
class Solution {
public:
    vector<vector<int> > permuteUnique(vector<int> &num) {
        vector<vector<int>> permutations;
        if(num.size() == 0)
            return permutations;
        vector<int> curr;
        vector<bool> isVisited(num.size(), false);
        /* we need to sort the input array here because of this array
           contains the duplication value, then we need to skip the duplication
           value for the final result */
        sort(num.begin(),num.end());
        dfs(permutations,curr,num,isVisited);
        return permutations;
    }

    void dfs(vector<vector<int>>& ret, vector<int> curr, vector<int> num, vector<bool> isVisited)
    {
        if(curr.size() == num.size())
        {
            ret.push_back(curr);
            return;
        }

        for(int i = 0; i < num.size(); ++i)
        {
            if(isVisited[i] == false)
            {
                isVisited[i] = true;
                curr.push_back(num[i]);
                dfs(ret,curr,num,isVisited);
                isVisited[i] = false;
                curr.pop_back();
                while(i < num.size()-1 && num[i] == num[i+1])
                //we use this while loop to skip the duplication value in the input array.
                    ++i;
            }
        }
    }
};
```
##N-Queens
经典的搜索题。

```c++
class Solution {
public:
    /**
     * Get all distinct N-Queen solutions
     * @param n: The number of queens
     * @return: All distinct solutions
     * For example, A string '...Q' shows a queen on forth position
     */
    vector<vector<string> > solveNQueens(int n) {
        // write your code here
        vector<vector<string>> rst;
        vector<string> cur(n, string(n, '.'));
        backtracking(rst, cur, 0);
        return rst;
    }

    void backtracking(vector<vector<string>>& rst, vector<string>& cur, int row){
        if (row == cur.size()){
            rst.emplace_back(cur);
            return;
        }

        for (int col = 0; col < cur.size(); col++){
            if (isValid(cur, row, col)){
                cur[row][col] = 'Q';
                backtracking(rst, cur, row + 1);
                cur[row][col] = '.';
            }
        }
    }

    bool isValid(vector<string> cur, int row, int col){
        for (int i = 0; i < row; i++){
            if (cur[i][col] == 'Q') return false;
        }
        for (int i = row - 1, j = col - 1; i >=0 && j >=0; i--, j--){
            if (cur[i][j] == 'Q') return false;
        }
        for (int i = row - 1, j = col + 1; i >=0 && j < cur.size(); i--, j++){
            if (cur[i][j] == 'Q') return false;
        }
        return true;
    }
};
```
##N-Queens II
只要求solutions的个数就可以。这道题并不用动归解，还是要用搜索。和上一题基本没差别。
```c++
class Solution {
public:
    /**
     * Calculate the total number of distinct N-Queen solutions.
     * @param n: The number of queens.
     * @return: The total number of distinct solutions.
     */
    int totalNQueens(int n) {
         // write your code here
        int rst = 0;
        vector<string> cur(n, string(n, '.'));
        backtracking(rst, cur, 0);
        return rst;
    }

    void backtracking(int& rst, vector<string>& cur, int row){
        if (row == cur.size()){
            ++rst;
            return;
        }

        for (int col = 0; col < cur.size(); col++){
            if (isValid(cur, row, col)){
                cur[row][col] = 'Q';
                backtracking(rst, cur, row + 1);
                cur[row][col] = '.';
            }
        }
    }

    bool isValid(vector<string> cur, int row, int col){
        for (int i = 0; i < row; i++){
            if (cur[i][col] == 'Q') return false;
        }
        for (int i = row - 1, j = col - 1; i >=0 && j >=0; i--, j--){
            if (cur[i][j] == 'Q') return false;
        }
        for (int i = row - 1, j = col + 1; i >=0 && j < cur.size(); i--, j++){
            if (cur[i][j] == 'Q') return false;
        }
        return true;
    }
};
```
