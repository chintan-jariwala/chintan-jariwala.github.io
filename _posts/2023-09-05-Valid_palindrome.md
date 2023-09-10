---
title: Trapping Rain Water
date: 2023-09-05 00:00:00 +0000
categories: [Coding, meta]
tags: [two-pointers, string, greedy]
author: jerry
---

## Problem
Given a string `s`, return `true` if the `s` can be palindrome after deleting at most one character from it.


### Example : 1
```textmate
Input: s = "aba"
Output: true
```

### Example: 2
```textmate
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

## Approach : 

The simplest approach could be check if the string is palindrome and the moment we find a place where it cannot be a palindrome, we explore deleting the element from either `left` or the `right` string

Example:
- `A`BC`A` == Match
- A`BC`A == Does not match 
  - We have 2 options to make it match. We can either remove `B` or we can remove `C`
  - Since there is no way to know which one will work. we have to try both

```java

```
