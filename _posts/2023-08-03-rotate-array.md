---
title: Rotate Array
date: 2023-08-03 00:00:00 +0800
categories: [Coding, Top150]
tags: [array, two-pointers]     # TAG names should always be lowercase
author: jerry
---


## Problem 

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

### Example 1:
```textmate
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

### Example 2:
```textmate
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

## Approach 1 : Reverse 

This approach is based on the fact that when we rotate the array `k` times, kk%nk elements from the back end of the array come to the front and the rest of the elements from the front shift backwards.

In this approach, we firstly reverse all the elements of the array. Then, reversing the first k elements followed by reversing the rest n−kn-kn−k elements gives us the required result.

```textmate
Original List                   : 1 2 3 4 5 6 7
After reversing all numbers     : 7 6 5 4 3 2 1
After reversing first k numbers : 5 6 7 4 3 2 1
After revering last n-k numbers : 5 6 7 1 2 3 4 --> Result
```

```java
public void rotate(int[] nums, int k) {
    // Input validation
    if (nums == null || nums.length == 0 || k < 0) {
        return;
    }

    // K can be a long value. So we mod it with length
    k = k % nums.length;

    // Reversing wroks
    swapFromBothSides(0, nums.length - 1, nums);
    swapFromBothSides(0, k - 1, nums);
    swapFromBothSides(k, nums.length - 1, nums);
}

private void swapFromBothSides(int start, int end, int[] nums) {
    while (start < end) {
        swap(start, end, nums);
        start++;
        end--;
    }
}

private void swap(int posA, int posB, int[] nums) {
    int temp = nums[posA];
    nums[posA] = nums[posB];
    nums[posB] = temp;
}
```
