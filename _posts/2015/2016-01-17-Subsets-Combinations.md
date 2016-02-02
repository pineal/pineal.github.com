---
layout: post
title: Subsets and Combinations
categories:
- Algorithm
tags:
- search
- DFS
- backtracking
- subsets
- combinations
---

# Subsets and Combinations

Original Questions on [LeetCode](http://www.lintcode.com):
- [Subsets I](http://www.lintcode.com/en/problem/subsets/)
- [Subsets II](http://www.lintcode.com/en/problem/subsets-ii/)
- [Combinations](http://www.lintcode.com/en/problem/combinations/)

##Subsets I

>Given a set of **distinct** integers, return all possible subsets.

Example
```
If S = [1,2,3], a solution is:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
###Recusive Solution
Backtracking的模板。

```c++
class Solution {
public:
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    vector<vector<int> > subsets(vector<int> &nums) {
    	// write your code here
    	vector<vector<int>> rst;
    	if (nums.size() == 0)
    	    return {};
    	vector<int> v;
    	sort(nums.begin(), nums.end());
    	backtracking(rst, v, nums, 0);
    	return rst;

    }


    void backtracking(vector<vector<int>>& rst, vector<int>& v, vector<int>& nums, int pos){

        rst.emplace_back(v);

        for (int i = pos; i < nums.size(); ++i){
            v.emplace_back(nums[i]);
            backtracking(rst, v, nums, i + 1);
            v.pop_back();
        }
    }
```


###Iterative Solution
建一个result的vector用来储存结果。初始化为一个［］。从nums取出下一个元素$$m$$，和result里所有的vector: $$ temp = v \in rst$$, 把 m 加到这些vector的末端: $$temp = temp \bigcup m$$, 再把这些vector放到result中，进行下一次迭代。一共有两重循环， 外面那层代表了每次从nums中取元素共有nums.size()个，里面那层代表把元素放到新的vector里，一共有result.size() 的次数。
```C++
class Solution {
public:
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    vector<vector<int> > subsets(vector<int> &nums) {
    	// write your code here
    	sort(nums.begin(),nums.end());
    	vector<vector<int>> rst(1);
    	vector<int> temp;
    	for (int i = 0; i < nums.size(); ++i){
    	  int size = rst.size();  
    	  for (int j = 0; j < size; ++j){
    	      temp = rst[j];
    	      temp.emplace_back(nums[i]);
    	      rst.emplace_back(temp);
    	      temp.clear();
    	  }
    	}
    	return rst;
    }
};
```

##Subsets II
上面那道题的followup，加上了去重的要求。

```C++
class Solution {
public:
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    vector<vector<int>> subsetsWithDup(const vector<int> &S) {
        vector<int> sorted_S(S);
        sort(sorted_S.begin(), sorted_S.end());
        vector<vector<int>> result(1);
        size_t previous_size = 0;
        for (size_t i = 0; i < sorted_S.size(); ++i) {
            const size_t size = result.size();
            for (size_t j = 0; j < size; ++j) {

                if (i == 0 || sorted_S[i] != sorted_S[i - 1] || j >= previous_size) {
                    result.emplace_back(result[j]);
                    result.back().emplace_back(sorted_S[i]);
                }
            }
            previous_size = size;
        }
        return result;
    }
};
```

去重的递归：

```c++
class Solution {
public:
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    vector<vector<int> > subsetsWithDup(const vector<int> &S) {
        // write your code here
        vector<vector<int>> rst;
        if (S.size() == 0)
            return {};
        vector <int> v;
        vector<int> nums(S);
        sort(nums.begin(), nums.end());
        backtracking(rst, nums, v, 0);
        return rst;
    }

    void backtracking(vector<vector<int>>& rst, vector<int>& nums, vector<int>& v, int pos){

        rst.emplace_back(v);    

        for (int i = pos; i < nums.size(); i++){
            v.emplace_back(nums[i]);
            backtracking (rst, nums, v, i + 1);
            v.pop_back();
            while(i < nums.size()-1 && nums[i] == nums[i+1]){
                ++i;
            }
        }
    }
};
```

##Combinations

相似的递归模版：

```c++
class Solution {
public:
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    vector<vector<int> > combine(int n, int k) {
        // write your code here
        vector<vector<int>> rst;
        vector<int> v;
        backtracking(n, k, rst, v, 1);
        return rst;
    }


    void backtracking(int n, int k, vector<vector<int>>& rst, vector<int>& v, int curr){
        if (v.size() == k){
            rst.emplace_back(v);
            return;
        }

        for (int i = curr;  i <= n; ++i){
            v.emplace_back(i);
            backtracking(n, k, rst, v, i+1);
            v.pop_back();
        }
    }
};

```
