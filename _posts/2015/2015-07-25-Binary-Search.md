---
layout: post
title: Binary Search
categories:
- Algorithm
tags:
- Binary Search
---

# Binary Search

## 经典二分搜索及其变种

```cpp
  int classic_binary_search(vector<int> input, int target) {
    if (input.empty()) {
      return -1;
    }
    int left = 0;
    int right = input.size() - 1;
    while (left <= right){
      int mid = left + (right - left)/2;
      if (input[mid] == target){
        return mid;
      }
      else if (input[mid] < target) {
        left = mid + 1;
      }
      else {
        right = mid - 1;
      }
    }
    return -1;
  }
};
```

```cpp
  int find_first_index(vector<int> input, int target) {
    // Write your solution here
    if (input.size() == 0) return -1;
    int left = 0, right = input.size() - 1;
    while (left < right - 1){
      int mid = left + (right - left)/2;
      if (target > input[mid]){
        left = mid;
      }
      else {
        right = mid;
      }
    }

    if (input[left] == target){
      return left;
    }
    else if (input[right] == target) {
      return right;
    }
    else
      return -1;
  }
```

```cpp
  int find_last_index(vector<int> input, int target) {
    if (input.empty())  {
      return -1;
    }
    int left = 0;
    int right = input.size() - 1;
    while (left < right - 1){
      int mid = left + (right - left)/2;
      if (target >= input[mid]){
        left = mid;
      }
      else {
        right = mid;
      }
    }

    if (input[right] == target) {
      return right;
    }
    else if (input[left] == target) {
      return left;
    }
    else {
      return -1;
    }
  }
```

## 二维二分搜索
### Search a 2D Matrix
从左到右递增，下一行的开头一定比上一行的末尾大。那么就可以转化为一维的二分搜索。考点在二维矩阵到一维矩阵的变换。

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
  int left = 0;
  int right = matrix.size() * matrix[0].size() - 1;
  while (left <= right) {
    int mid = left + (right - left) / 2;
    int i = mid / matrix[0].size();
    int j = mid % matrix[0].size();
    if (matrix[i][j] == target) {
      return true;
    } else if (target > matrix[i][j]) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  return false;
}
```

### Search a 2D matrix II
从左往右一定递增，从上到下一定递增。跟之前一题相比，这里的二维矩阵并不能保证转化成一维递增的矩阵。
如果从左上角开始往右下角搜索，那么有多种可能，刚开始用的是分治法，把矩阵分成四块，由于这个矩形的性质，我们只能排除掉一块，然后往三块可能的继续搜。时间复杂度为O(n^1.58), 参考分治法时间复杂度分析公式.
但如果从右上角往左下角搜，那么可以保证向左一定是递减的，向下一定是递增的，那么可以排除掉特定行或者特定列。这样的时间复杂度出来的是O(m + n).

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
	int m = matrix.size();
	if (m == 0) return false;
	int n = matrix[0].size();
	int i = 0, j = n - 1;
	while (i < m && j >= 0) {
		if (matrix[i][j] == target)
			return true;
		else if (matrix[i][j] > target) {
			j--;
		} else
			i++;
	}
	return false;
}
```
