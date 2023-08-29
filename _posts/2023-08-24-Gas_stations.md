---
title: Gas Stations
date: 2023-08-24 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, greedy]
author: jerry
---

## Problem

There are `n` gas stations along a circular route, where the amount of gas at the ith station is `gas[i]`.

You have a car with an **unlimited gas tank** and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the **starting gas station's index** if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

### Example : 1
```textmate
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

### Example : 2
```textmate
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

## Approach : 1 

To understand this question - Lets consider this 

Example - 
```textmate
gas =  [7, 1, 0, 11, 4]
cost = [5, 9, 1,  2, 5]
```

// TODO : Coming soon


```java
public class Solution {

  public static void main(String[] args) {
    new Solution().canCompleteCircuit(new int[]{1,2,3,4,5}, new int[]{3,4,5,1,2})
  }

  public int canCompleteCircuit(int[] gas, int[] cost) {
    
    // total gas 
    int n = gas.length;
    // How much gas will we be left with once we finish the entire trip ?
    int total_left = 0;
    // At each stage, how much did we use ?
    int left = 0;
    // Where do we start from ? Initially we start from 0
    int start = 0;

    for (int i = 0; i < n; i++) {
      
      // We calculate the usage total
      total_left += (gas[i] - cost[i]);
      // Similarly we calcuate for each stop
      left += (gas[i] - cost[i]);
      
      // At this point, if we are left with -nve gas, then that will be our new start
      if (left < 0) {
        left = 0;
        start = i + 1;
      }
    }
    
    // If here, our total left is -Ne then we cannot complete the loop
    if (total_left < 0) {
      return -1;
    } else {
      return start;
    }
  }
}
```
