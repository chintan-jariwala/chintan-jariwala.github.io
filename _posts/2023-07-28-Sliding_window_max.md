---
title: Sliding Window Maximum
date: 2023-07-28 00:00:00 +0800
categories: [Coding, Top100]
tags: [sliding-window, array-dqueue]     # TAG names should always be lowercase
author: jerry
---

## Problem

You are given an array of integers `nums`, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

#### Example 1:
```textmate
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
#### Example 2:
```textmate
Input: nums = [1], k = 1
Output: [1]
```

#### Approach
- We use a `Dequeue` datastructure which is basically a doubly LL that can add and remove from both ends in `O(1)` time
- We will keep `Max` elements in the front and others after that.
- Few things to consider as we iterate though the loop
  - Keep the window in check. If window size is `2 elements` then that is the first thing we check. If we exceed the size then we remove from front.
  - If we find an element which is bigger than out last element in the queue. Then we remove that
  - We add element in the queue.
  - Start populating the result once we reach the first end of the window

#### Solution

##### TC = `O(n)`
##### SC = `O(k)`

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0)
        return nums;

    // Create a dequeue since we use adding to front and back. We store the indexes of elements
    Deque<Integer> deque = new ArrayDeque<>();

    int n = nums.length;
    int[] res = new int[n - k + 1];
    int right = 0;

    for (int i = 0; i < nums.length; i++) {
        int num = nums[i];

        // Clean up the queue if we have more than 3 elements in the queue - 0 [1 2 3] 4
        if (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }

        // We append new data at the end. so if the last
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }

        deque.add(i);
        if (i >= k - 1) {
            res[right++] = nums[deque.peekFirst()];
        }
    }
    return res;
}
```
