---
title: Buildings With An Ocean View
date: 2023-08-28 00:00:00 +0000
categories: [Coding, Top150]
tags: []
author: jerry
---

## Problem

Given the `root` of a binary tree, return **the vertical order traversal** of its nodes' values. (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from `left` to `right`.

### Example : 1
```textmate
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation:
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.
```

### Example: 2
```textmate
Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.
```

### Example: 3
```textmate
Input: root = [1,2,3,4,6,5,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.
```

## Approach : 1 

The approach is to ensure when traversing through the tree 
- Starting fromt he root,(We consider root as 0)
- We go left - we subtract 1 from root's position
- We go right - we add 1 to root's position
- Keep doing this, and we will have a `position` associated with each node

In the end we just combine the nodes which are at the same position

```java
public class BinaryTreeVerticalOrderTraversal {

    public static void main(String[] args) {

        TreeNode root = TreeNode.CreateTree(Arrays.asList(3,9,20,null,null,15,7));

        List<List<Integer>> lists = new BinaryTreeVerticalOrderTraversal().verticalOrder(root);
        System.out.println(lists);
    }

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
            heightMap.computeIfAbsent(current.height, val -> new ArrayList<>()).add(current.node.val);

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

        for (int i = left; i <= right; i++) {
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
