---
title: Best time to buy and sell stock
date: 2023-08-04 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, two-pointers, dynamic-programming]
author: jerry
---

## Problem

You are given an array `prices` where `prices[i] `is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different** day in the future to sell that stock.

Return the **maximum profit** you can achieve from this transaction. If you cannot achieve any profit, return 0.

### Example : 1
```textmate
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

### Example : 2
```textmate
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

### Constraints:
- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

## Approach 1: Brute force
The goal here is to find the maximum difference between 2 numbers from left to right since time only moves in 1 direction

Since we can either buy or sell on a given day we only will do 1 calculation or won't do anything at all

```java
public int maxProfitBruteForce(int[] prices) {

    int maxProfit = 0;

    // Input validation
    if (prices == null || prices.length == 0) {
        return maxProfit;
    }

    // Check the diff between 2 values from left to right  !
    for (int i = 0; i < prices.length; i++) {
        for (int j = i+1; j < prices.length; j++) {
            int diff = prices[j] - prices[i];
            maxProfit = Math.max(maxProfit, diff);
        }
    }

    return maxProfit;
}
```

## Approach 2: One pass

Since we are doing only 1 transactions per day, we can
- Keep track of the lowest price we have seen so far,
  - If current price is lower than what we have seen so far, we buy that day
  - else we sell and calculate the profit

```java
public int maxProfitOnePass(int[] prices) {

    int maxProfit = 0;

    // Input validation
    if (prices == null || prices.length == 0) {
        return maxProfit;
    }

    // Track the lowest price we saw so, far
    int minPrice = Integer.MAX_VALUE;

    // Check the diff between 2 values from left to right  !
    for (int price : prices) {
        // if today's price is lower than our min price we need to buy today
        if (price < minPrice) {
            minPrice = price;
        } else {
            // here we can see if we sell today, how much profit can we get
            int diff = price - minPrice;
            maxProfit = Math.max(diff, maxProfit);
        }
    }

    return maxProfit;
}
```
