---
layout: post
title: Binary Search
categories:
- Algorithm
tags:
- Algorithm
- Binary Search
---

##￼Classical Binary Search
Given an sorted integer array-nums, and an integer target. Find the any or first or last position of target in nums, return -1 if target dose not exsit.

###Recursion or While-Loop?

###A generic binary search template

The answer refers to [九章算法](http://www.jiuzhang.com/solutions/binary-search/).

```python
class Solution:

    # @param nums: The integer array
    # @param target: Target number to find
    # @return the first position of target in nums, position start from 0

    def binarySearch(self, nums, target):
        if len(nums) == 0:
            return -1

        start, end = 0, len(nums) - 1
        while start + 1 < end:
					#why start + 1 < end -> avoid dead loop
            mid = (start + end) / 2
						#In Java/C++ with int type ->
						#start + (end - start) / 2 ->
						#avoid stack overflow ->
						#zhuangbility

            if nums[mid] < target:
                start = mid
            else:
                end = mid

				#return the first ? last ? any?
        if nums[start] == target:
            return start
        if nums[end] == target:
            return end
        return -1
```
