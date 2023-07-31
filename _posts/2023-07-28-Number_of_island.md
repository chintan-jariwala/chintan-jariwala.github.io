---
title: Number of Islands
date: 2023-07-28 00:00:00 +0800
categories: [Coding, Top100]
tags: [bfs, breath-first-search, dfs, depth-first-search, graph, medium]     # TAG names should always be lowercase
author: jerry
---

## Problem

Given an m x n 2D binary grid `grid` which represents a map of '1's (land) and '0's (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

#### Example 1:
```textmate
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```
#### Example 2:

```textmate
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

#### Approach
- Simple traversal of the 2d array
- As we are traversing, we check each value. If we see ground `1` anywhere, we start our DFS and turn each `1` to `0`. Once we are done. we count that as 1.
- We increment our parent loop by 1

> Here, we are changing the original input. If we cannot change, then
> - We can have a visited map -> increases our SC by `O(nm)`
> - We can change the value to something else temporarily. like - `-1` and once we're done we change it back to its original format.


#### Solution

##### TC = `O(M x N)`
##### SC = `O(M x N) (Worst case our DFS recursion stack will have everything in it if we have all 1s)`

```java
class Solution {
    public int numIslands(char[][] grid) {
        int island = 0;

        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] == '0')
                    continue;
                helper(grid, i, j);
                island++;
            }
        }
        return island;
    }
    
    private void helper(char[][] grid, int i, int j) {
        // Checking for boundries and checking if we have visited the node before
        if (i < 0 || i > grid.length - 1 || j < 0 || j > grid[0].length - 1 || grid[i][j] == '0') {
            return;
        }

        grid[i][j] = '0';
        helper(grid, i + 1, j);
        helper(grid, i - 1, j);
        helper(grid, i, j + 1);
        helper(grid, i, j - 1);
    }
}
```
