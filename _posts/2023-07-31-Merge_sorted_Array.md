---
title: Merge Sorted Array
date: 2023-07-31 00:00:00 +0800
categories: [Coding, Top150]
tags: [array, sorting, two-pointers]     # TAG names should always be lowercase
author: jerry
---

## Problem

You are given two integer arrays `nums1` and `nums2`, sorted in `non-decreasing` order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array `sorted in non-decreasing` order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

### Example 1 :
```textmate
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

### Example 2 :
```textmate
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

### Example 3 :
```textmate
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

## Approach 1 : Merge normally by two pointers

Here, Initially we may think to start from the left end of both the arrays, but its not possible since we need to return the solution in one of the input arrays. So if we start from the left, we will end up overwriting the values.

The approach is to have 2 pointers at the end of each line and start comparing

Eg

```textmate
1 2 3 0 0 0         2 5 6
    A     C             B
```


3 pointers, `A` , `B` and `C` are initialized on the locations given above.

We compare `A` and `B` which is `3 > 6` => `false` => so we put `6` in the answer and decrease the pointer

```textmate
1 2 3 0 0 6         2 5 6
    A   C             B
```

Continue doing so until we find a solution

```java
    public void merge(int[] nums1, int m, int[] nums2, int n) {

        // Input validation
        if (nums1 == null || nums2 == null || nums1.length < m + n) {
            return;
        }

        // Initialize the pointers
        int c = nums1.length - 1;
        int a = m-1;
        int b = n-1;

        // We continue comparing as far as we can
        while (a >= 0 && b >= 0) {
            if (nums1[a] > nums2[b]) {
                nums1[c] = nums1[a];
                a--;
            } else {
                nums1[c] = nums2[b];
                b--;
            }
            c--;
        }
        // we put all the remaining bits from both the arrays
        while (a >= 0) {
            nums1[c--] = nums1[a--];
        }
        while (b >= 0) {
            nums1[c--] = nums2[b--];
        }
    }
```

### TC : `O(m+n) `

### SC : `O(1)`

