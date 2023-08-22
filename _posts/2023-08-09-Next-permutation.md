---
title: Next Permutation
date: 2023-08-09 00:00:00 +0000
categories: [Coding, Top150]
tags: [math, greedy]
author: jerry
---

## Problem

A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.
- For example, for arr = `[1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1].`

The **next** **permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of **arr** = `[1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of **arr** = `[2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, find the next permutation of `nums`.

The replacement must be _in place_ and use only constant extra memory.

### Example : 1
```textmate
Input: nums = [1,2,3]
Output: [1,3,2]
```

### Example : 2
```textmate
Input: nums = [3,2,1]
Output: [1,2,3]
```

### Example : 3
```textmate
Input: nums = [1,1,5]
Output: [1,5,1]
```

### Constraints:
- `0 <= num <= 108`

## Approach : 1 Greedy

Lets do this with an example,

`1 2 3` -> `1 3 2`
`1 3 2` -> `2 1 3`

In both the examples above, We are first finding the increasing sequance from the left side,



```java

```

