---
layout: post
title: Binary Search
categories:
- Algorithm
tags:
- Algorithm
- Binary Search
---

## ￼Classical Binary Search
Given an sorted integer array-nums, and an integer target. Find the any or first or last position of target in nums, return -1 if target dose not exsit.

~~~cpp
class Solution {
 public:
  int solve(vector<int> input, int target) {
    if (input.size() == 0) return -1;
    int left = 0, right = input.size() - 1;
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
~~~

## Find first / last element by binary search
变种。记住就好。
~~~cpp
class Solution {
 public:
  int firstOccur(vector<int> input, int target) {
    // Write your solution here
    if (input.size() == 0) return -1;
    int left = 0, right = input.size() - 1;
    while (left < right - 1){
      int mid = left + (right - left)/2;
      if (target > input[mid]){
    //if (target >= input[mid]){  //find last
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
    //find last element
    //if (input[right] == target) return right;
    //else if (input[left] == target) return left;    
    return -1;
  }
};
~~~
