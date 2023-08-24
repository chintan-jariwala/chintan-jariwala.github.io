---
title: Product Of Array Except Self
date: 2023-08-24 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, prefix-sum]
author: jerry
---

## Problem

Given an integer array `nums`, return an `array` answer such that `answer[i]` is equal to the `product of all the elements` of nums `except nums[i]`.

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

**You must write an algorithm that runs in O(n) time and without using the division operation.**

### Example : 1
```textmate
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

### Example : 2
```textmate
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

### Constraints:
- `2 <= nums.length <= 105`

## Approach : 1 

The entire problem becomes much easier if we could just do division (DUH)

- Lets assume we have this input `[3, 2, 5]`
- Technically the answer here is - `[10, 15, 6]`

**If we can perform division then the approach becomes** 
- Multiply every element int he array and get a number `Total = 3 * 2 *5` => `Total = 30`
- Start diving the `Total` by each element and that is the answer
  - eg `[30/3, 30/2, 30/5]` => `[10, 15, 6]`

**However, Since we cannot do this, another approach can be `Pre calculating the multiplications`**

- Assuming for the same input `[3, 2, 5]` if we calculate the multiplication of everything on the left side and multiplication of everything on the right side that would be enough.
  - Example : For `[3, 2, 5]` 
  - ```textmate
                            <-L->   <-R->
    To find the result for [_ _ _ 2 _ _ _]
    We can get the answer quickly if we have
    L => Multiplication of all elements before 2
    R => Multiplication of all elements after 2
    
    So, the result for the index 2 -> L * R
    ```
  - To implement this, we need to create 2 arrays 
    1. For Multiplication of Left side elements
    2. For Multiplication of Right side elements
  - It can be simplified further by using the resulting array to store one of the multiplication arrays

```java
public int[] productExceptSelf(int[] nums) {

  // Basic Input validation
  if(nums == null || nums.length == 0) {
    return nums;
  }

  // Create an array for result
  int[] res = new int[nums.length];

  // calculate the left side multi first
  int[] left = new int[nums.length];

  for (int i = 0; i < nums.length; i++) {
    // For the first one we don't have any values on the left so we assume 1
    if (i == 0) {
      left[i] = 1;
    } else {
      // Multiply the
      left[i] = left[i-1] * nums[i-1];
    }
  }

  // similarly for the right
  int[] right = new int[nums.length];


  for (int i = nums.length - 1; i > -1; i--) {
    // for the last index we don't have anything on the right
    if (i == nums.length - 1) {
      right[i] = 1;
    } else {
      right[i] = right[i+1] * nums[i+1];
    }
  }

  // Get the result by left * right
  for (int i = 0; i < nums.length; i++) {
    res[i] = left[i] * right[i];
  }

  return res;
}
```
