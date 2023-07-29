---
title: Next Permutation
date: 2023-07-28 00:00:00 +0800
categories: [Coding, Top100]
tags: [mathematics, array]     # TAG names should always be lowercase
author: jerry
---

## Problem

A `permutation` of an array of integers is an arrangement of its members into a sequence or linear order.

Example - `123` => `132` => `213` => `231` => `312` => `321`  => `123`

The `next permutation` of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the `next permutation` of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of arr = `[1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of arr = `[2,3,1]` is `[3,1,2]`.
- While the next permutation of arr = `[3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.


#### Example 1:
```textmate
Input: nums = [1,2,3]
Output: [1,3,2]
```
#### Example 2:
```textmate
Input: nums = [3,2,1]
Output: [1,2,3]
```

#### Approach
- The approach here is to understand the pattern we are going for.
- Looking at this - `123` => `132` => `213` => `231` => `312` => `321`  => `123` we know that we are looking for the next immediate update.

Here is the algorithm
- We tackle this from the right most side and look for an increasing sequence
- eg. `123` => from the right we find `23`<br/>
  eg. `132` => from the right we find `12`<br/>
  eg. `15681` => from the right we find `68`<br/>
- Once we find that we have 1 element that we can swap. <br/>in the case of `15681` it is `6`<br/>
  in case of `132` it is `1`
- Next, we again check from the right end and find an element with is greater than our current element
  <br/> In case of `132` it is `2` since `1`( current) < `2`(greater)
  <br/> In case of `15681` it is `1` since `6`(current) < `8`(greater)
- We swap them. and we get
  <br/> In case of `132` => `231`
  <br/> In case of `15681` => `15861`
- Last thing we need is to reverse the array after our current element
  <br/> In case of `231` => reverse after `2` => `213`
  <br/> In case of `15861` => reverse after `8` => `15816`

#### Solution

##### TC = `O(n)`
##### SC = `O(1)`

```java
public int[] nextPermutation(int[] nums) {

    // Input validation
    if (nums == null || nums.length == 0) {
        return nums;
    }

    // Check from the right to left and find an increasing sequence
    int i = nums.length-2;
    while (i > 0 && nums[i] >= nums[i+1]) {
        i--;
    }
    int j;
    if (i >= 0) {
        j = nums.length-1;
        while (j >= 0 && nums[j] <= nums[i]) {
            j--;
        }
        swap(nums, i, j);
    }
    reverse(nums, i+1, nums.length-1);

    return nums;
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        swap(nums, start++, end--);
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```
