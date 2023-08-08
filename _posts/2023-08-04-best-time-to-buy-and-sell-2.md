---
title: Best time to buy and sell stock II
date: 2023-08-04 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, two-pointers, dynamic-programming]
author: jerry
---

## Problem

You are given an integer array prices where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to **buy and/or sell** the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.
### Example : 1
```textmate
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

### Example : 2
```textmate
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```

### Constraints:
- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

## Approach : One pass 

Given that we can buy and sell on the same day, we need to know wether to buy again on a given day or not.

To look ahead, we can have 2 pointers like 

```
7 1 5 3 6 4
i j
```

Since j is ahead, we know if we want to buy on ith day or not.

```java
public int maxProfitOnePass(int[] prices) {

  int maxProfit = 0;
  
  // Input validation
  if (prices == null || prices.length == 0) {
    return maxProfit;
  }
  
  for (int i = 0, j = 1; j < prices.length; i++, j++) {
    if (prices[i] < prices[j]) {
        maxProfit = maxProfit + prices[j] - prices[i];
    }
  }
  return maxProfit;
}
```

This works because we can only buy 1 share.

What if we could buy 2 shares ? 
- We multiply the difference by 2
