---
title: Best time to buy and sell stock III
date: 2023-08-04 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, two-pointers, dynamic-programming]
author: jerry
---

## Problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### Example : 1
```textmate
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

### Example : 2
```textmate
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

### Example : 3
```textmate
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

### Constraints:
- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

## Approach : 1 DP

- We need to check every set of differences and find the max.
But We can be smart about this, Technically on any given day you have 2 options

Options for transactions : 
- Either you don't do anything on that day
- Best we can do by selling on that day (But wait, When did we buy ?)
  - We need to buy on any day before the day we sell. 

```textmate
                       -- T[Transaction][day - 1] - We don't do anything
T[Transaction][day] --|
                       -- We sell on this day but we sell to maximize the profit
                         So, We need to check that for all days before today
                         (price[day] - price[i] ) + T[Transaction - 1][i]
                         i = 0 -> day
```

```java
public int maxProfitOnePass(int[] prices) {

  int maxProfit = 0;
  
  int transactions = 2;
  
  int[][] dp = new int[transactions + 1][prices.length];
  
  // Input validation
  if (prices.length == 0) {
    return maxProfit;
  }
  
  for (int k = 1; k < dp.length; k++) {
    // We buy on day 0
    int maxDiff = -prices[0];
    for (int i = 1; i < dp[0].length; i++) {
      // Check how much we can make if we did nothing
      dp[k][i] = Math.max(dp[k][i-1], prices[i] + maxDiff);
      // The max money we can make is, either we do a transaction vs what we already have
      maxDiff = Math.max(maxDiff, dp[k-1][i] - prices[i]);
    }
  }
  
  return dp[transactions][prices.length-1];
}
```




