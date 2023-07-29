---
title: Add Two Numbers
date: 2023-07-27 12:33:00 +0800
categories: [Coding, Top100]
tags: [linkedlist, recursion]     # TAG names should always be lowercase
author: jerry
---

## Problem
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

#### Example 1:
![Add two numbers.png](/assets/img/add_two_numbers.png)
_Example 1_
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
#### Example 2:
```
Input: l1 = [0], l2 = [0]
Output: [0]
```

#### Approach - 1 

- Just basics of linked list,
- The tricky part here is to add the carry, we need to ensure once, we add two numbers and total goes above 9 then we have to split this number and add it to the next. 

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode();
        ListNode cur = dummy;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            int x = l1 != null ? l1.val : 0;
            int y = l2 != null ? l2.val : 0;

            int sum = x + y + carry;

            carry = sum / 10;
            cur.next = new ListNode(sum % 10);
            cur = cur.next;

            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;

        }
        return dummy.next;
    }
```

##### TC : `O(1)` : Technically its O(n) since we need to iterate through the list
##### SC : `O(1)`
