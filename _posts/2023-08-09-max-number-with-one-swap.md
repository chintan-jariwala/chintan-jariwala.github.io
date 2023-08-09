---
title: Maximum Swap
date: 2023-08-09 00:00:00 +0000
categories: [Coding, Top150]
tags: [math, greedy]
author: jerry
---

## Problem

You are given an integer `num`. You can swap `two digits` at most once to get the **maximum** valued number.

Return the **maximum** valued number you can get.

### Example : 1
```textmate
Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```

### Example : 2
```textmate
Input: num = 9973
Output: 9973
Explanation: No swap.
```

### Constraints:
- `0 <= num <= 108`

## Approach : 1 Greedy

- We have a number which can look like : ` 2 7 3 6`
- From this, the intuition is to find a number which is greater than the number on the left and move it for example  we move `6` to first position which will give us 
` 6 7 3 2`.
- However, This is not the largest number that we can get with one swap.

> Note : If the entire array is in decreasing order, we cannot increase the array by swapping anything. eg `3 2 1` : here we cannot swap anything

So, 
- first we need to find the first point where the array is actually increasing.
- Then we look for the max element  on the right side of the increasing point
- 

```java
public int maximumSwap(int num) {

  if (num == 0)
    return 0;
  
  // Convert num to char array
  char[] input = String.valueOf(num).toCharArray();
  
  // We find the first point where the numbers are not decreasing
  // assuming the point is at index 0;
  int point = 0;
  
  // We compare the first 2 numbers until we find the point
  while (++point < input.length && input[point -1] >= input[point]);
  
  // If there is no increasing pair, we will reach the end of array and we cannot do anything
  if (point == input.length) return num;
  
  // Now we need to find max on the right side
  char max = '0';
  int maxPos = point;
  
  // Here we ensure that at the maxPos we have the max value on the right side of the point. So we compare
  for (int i = point; i < input.length; i++) {
    // We use `<=` here because we want to go to the right most in the case of 199 -> so the swap can be 991 instead of 919
    if (max <= input[i]) {
      max = input[i];
      maxPos = i;
    }
  }
  
  // Now to maximize the number we have to replace it with the smaller number which appears earlier on the left
  for (int i = 0; i < point; i++) {
    if (max > input[i]) {
      // Swap the values
      char temp = input[i];
      input[i] = input[maxPos];
      input[maxPos] = temp;
      // No point in going further, we return here
      return Integer.parseInt(String.valueOf(input));
    }
  }
  
  return num;
}
```

