---
title: Buildings With An Ocean View
date: 2023-08-28 00:00:00 +0000
categories: [Coding, Top150]
tags: []
author: jerry
---

## Problem
There are `n` buildings in a line. You are given an **integer array heights** of size `n` that represents the heights of the buildings in the line.

The ocean is **to the right** of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a smaller height.

Return a list of indices `(0-indexed)` of buildings that have an ocean view, sorted in increasing order.


### Example : 1
![b-tree-1.png](/assets/img/b_tree-1.png)
```textmate
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
```

### Example: 2
![b-tree-2.png](/assets/img/b_tree-2.png)
```textmate
Input: root = [3,9,8,4,0,1,7]
Output: [[4],[9],[3,0,1],[8],[7]]
```

### Example: 3
![b-tree-3.png](/assets/img/b_tree-3.png)
```textmate
Input: root = [3,9,8,4,0,1,7,null,null,null,2,5]
Output: [[4],[9,5],[3,0,1],[8,2],[7]]
```

## Approach : 1 

Code simplifies it

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
  public List<List<Integer>> verticalOrder(TreeNode root) {
    // Basic validation
    if (root == null) {
      return null;
    }

    if (root.left == null && root.right == null) {
      return List.of(List.of(root.val));
    }

    // Res
    List<List<Integer>> res = new ArrayList<>();

    // Grouping nodes by height
    Map<Integer, List<Integer>> heightMap = new HashMap<>();


    // Queue for traversal with BFS
    Queue<Pair> queue = new LinkedList<>();
    // Lets have a left most and right most
    int left = 0;
    int right = 0;

    // Add root to the node
    queue.add(new Pair(root, 0));

    while (!queue.isEmpty()) {
      Pair current = queue.poll();

      // do we have that in the map ?
      heightMap.getOrDefault(current.height, new ArrayList<>()).add(current.node.val);

      // Update our left and right
      left = Math.min(left, current.height);
      right = Math.max(right, current.height);

      if (current.node.left != null) {
        queue.add(new Pair(current.node.left, current.height - 1));
      }

      if (current.node.right != null) {
        queue.add(new Pair(current.node.right, current.height + 1));
      }
    }

    for (int i = left; i < right; i++) {
      res.add(heightMap.get(i));
    }
    return res;
  }

  static class Pair {
    TreeNode node;
    int height;

    public Pair(TreeNode node, int height) {
      this.node = node;
      this.height = height;
    }
  }
}
```
