---
title: Trapping Rain Water
date: 2023-08-28 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, two-pointers, dynamic-programming, stack, monotonic-stack]
author: jerry
---

## Problem
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.


### Example : 1
![RainWaterTrap.png](/assets/img/rainwatertrap.png)
```textmate
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### Example: 2
```textmate
Input: height = [4,2,0,3,2,5]
Output: 9
```

## Approach : 1 

In order to trap water, we need to check if the current position is between 2 positions which are higher than that.

So, to achieve this, we 
- Start from both the ends of the list

Example:
```textmate

Ex : [3, 2, 1, 4]
             ___  
 ___        |   |
|   |___    |   |
|   |   |___|   |
|___|___|___|___|
  3   2   1   4
From the looks of this, we can fill water above- 2, 1,
```
Simulation of above example:
- Start with `left = 0` and `right = len - 1`
- We compare both the points `3` and `4`
  - we go to the lesser number because that will spill water
- Calculate how much water this "bucket" can hold ?
  - In our example, we know `3` < `4` so we check if `3` can hold water above it,
    - To check this, we need a building on left of `3` to support the spilling from left side. (The spilling on right is already supported because `4` is greater than `3`)
    - If we have the building we calculate the water storage, or if we don't our current building is the support on the left side
We continue this until we meet in the midle


```java
public class TrappingRainWater {

    public static void main(String[] args) {
        new TrappingRainWater().trap(new int[]{0,1,0,2,1,0,1,3,2,1,2,1});
    }

    public int trap(int[] height) {
        // Base condition check
        if (height == null || height.length == 0)
            return -1;

        // the max height we have seens so far on left
        int leftMax = 0;
        // the max height we have seen so far on right
        int rightMax = 0;
        // starting point on the left
        int l = 0;
        // starting point on the right
        int r = height.length-1;
        // Area calculated so far
        int area = 0;

        while (l<r) {
            // Check which builing is shorter
            if (height[l] <= height[r]) {
                // Check if there is a support on the left side already
                if (height[l] >= leftMax) {
                    // If no support, we create one
                    leftMax = height[l];
                } else {
                    // If support is there we calculate the answer and move forward
                    area += (leftMax - height[l]);
                }
                l++;
            } else {
                if (height[r] >= rightMax) {
                    rightMax = height[r];
                } else {
                    area += (rightMax - height[r]);
                }
                r--;
            }
        }
        return area;
    }
}
```
