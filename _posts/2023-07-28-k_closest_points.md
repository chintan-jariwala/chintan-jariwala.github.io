---
title: K Closest points to origin
date: 2023-07-28 00:00:00 +0800
categories: [Coding, Top100]
tags: [priority-queue, heap]     # TAG names should always be lowercase
author: jerry
---

## Problem
Given an array of points where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer k, return the `k closest points to the origin (0, 0)`.

The distance between two points on the X-Y plane is the `Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2)`.

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

#### Example 1:
```textmate
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```
#### Example 2:
```textmate
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

#### Approach
##### Heap
- Using a heap we can add all the elements in it. and have a custom comparator in it
- Once we are done, we can Get K elemetns out and that is the result

##### QuickSelect

- We take low and high points - Which are initially `0` and `last`
- We `randomly` select an element called pivot - in this prog we select the `last` element as pivot
- We also need another element lets call it `j`. Initially `j`=low
- we start comparing elements in the array to this pivot
  - If the current element is more than pivot, we continue
  - If current element is less than or equal to pivot we swap it with the `j` element.
  - Once we are done with the loop, we put `pivot` in its right position which is `j`'s position.
  - Here all elements on the left of `pivot` are less than pivot and all on the right are more.
- The idea is to keep doing this until we find a `pivot` which is `k`
- The solution below has comments which will help.

#### Solution

##### TC = `O(n2)`
##### SC = `O(1)`

```java
public int[][] kClosestQuickSelect(int[][] points, int k) {
    // Using a quick select algorithm here.
    // Take the low and high points and place the 1 element in the right list
    int low = 0;
    int high = points.length - 1;

    while (low < high) {
        int mid = partition(points, low, high);
        // from quick select - everything at the left of mid is less than mid. So we check,
        // if mid is less than K like k = 3 and {1,"3", 8,9}
        if (mid < k) {
            // We need to sort more on the right, move low to the right side
            low = mid + 1;
        } else if (mid > k) {
            // We need to sort more on the left, move high to the left side
            high = mid - 1;
        } else {
            // mid is exactly where it should be. we break
            break;
        }
    }
    return Arrays.copyOfRange(points, 0, k);
}

private int partition(int[][] points, int low, int high) {
    // Select an index at the start
    int j = low;
    // Choose a pivot. Can choose anything, This program chooses the last element
    int[] pivot = points[high];
    // Iterate though the array between the ranges excluding the last element as that is pivot
    for (int i = low; i < high; i++) {
        if (isCurrentPointCloserThanPivot(points[i], pivot)) {
            // If the current point is closer than the pivot point
            // We swap the current point with the j
            swap(points, i, j);
            j++;
        }
    }
    // Once this loop is done, we have the correct position of pivot is `j`
    swap(points, j, high);
    return j;
}

private void swap(int[][] points, int i, int j) {
    int[] temp = points[i];
    points[i] = points[j];
    points[j] = temp;
}

private boolean isCurrentPointCloserThanPivot(int[] point, int[] pivot) {
    int distanceOfPoint = (point[0] * point[0]) + (point[1] * point[1]);
    int distanceOfPivot = (pivot[0] * pivot[0]) + (pivot[1] * pivot[1]);

    // We return true if the distance of point is lesser or equal to pivot
    return distanceOfPoint <= distanceOfPivot;
}

public int[][] kClosestUsingHeap(int[][] points, int k) {

    if (points == null || points.length == 0)
        return points;

    // Create a PQ to hold the top k elements to return
    Queue<int[]> queue = new PriorityQueue<>((o1, o2) -> {
        int distance1 = (o1[0] * o1[0]) + (o1[1] * o1[1]);
        int distance2 = (o2[0] * o2[0]) + (o2[1] * o2[1]);
        return Integer.compare(distance1, distance2);
    });
    Collections.addAll(queue, points);

    int[][] res = new int[k][2];
    int i = 0;

    while (k-- > 0) {
        int[] p = queue.poll();
        res[i][0] = p[0];
        res[i][1] = p[1];
        i++;
    }

    return res;
}
```
