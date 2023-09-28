---
title: Reverse Linked List
date: 2023-09-28 00:00:00 +0000
categories: [Coding, apple, microsoft]
tags: [Linked-list]
author: ushah
---

## Problem
Given the head of a singly linked list, reverse the list, and return the reversed list.

![reverseLinkedList_1.JPG](/assets/img/reverseLinkedList_1.JPG)
### Example : 1
```textmate
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

![reverseLinkedList_1.JPG](/assets/img/reverseLinkedList_2.JPG)
### Example: 2
```textmate
Input: head = [1,2]
Output: ["2,1]
```
### Example : 3
![reverseLinkedList_3.JPG](/assets/img/reverseLinkedList_3.JPG)
```textmate
Input: head = []
Output: []
```


### Constrain
* The number of nodes in the list is the range [0, 5000].
* -5000 <= Node.val <= 5000

### Follow up: 
A linked list can be reversed either iteratively or recursively. Could you implement both?

## Approach :

The simplest way a linked list can be reversed is by using the temporary variable method. This is similar to switching 
variable values using the third temporary variable. This approach is iterative one, where we store the value of the next
node to a temp one to use it later on.



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

 class Solution {
     public ListNode reverseList(ListNode head) {
         // We initialize the pointer for traversing one as null since it's empty 
         // another one to head to start traversing the linked list.
         ListNode prev = null;
         ListNode current = head;

         while (current != null) {
             // We can change the current node's next pointer to point to its previous element.
             // A node does not have reference to its previous node,
             // we must store its previous element beforehand.
             // Another thing to keep in mind, when we are done traversing and reach to the 
             // end of linked list. We need to return the previous pointer since current pointer is replaced with
             // null pointer at the end.
             ListNode next = current.next;
             current.next = prev;
             prev = current;
             current = next;
         }
        return prev;
     }
}
```
