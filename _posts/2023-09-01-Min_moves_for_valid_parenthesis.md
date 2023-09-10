---
title: Trapping Rain Water
date: 2023-09-01 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, two-pointers, dynamic-programming, stack, monotonic-stack]
author: jerry
---

## Problem
Given a string s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.

Formally, a parentheses string is valid if and only if:

- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

### Example : 1
```textmate
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

### Example: 2
```textmate
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

### Example: 3
```textmate
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

## Approach : 1 

To figure out all the places where the `balance of the brackets` are breaking, we need to find the place where the balance breaks
This is pretty simple 
- Initialize a stack
- Start iterating from l - R
- If we see an `(` we push to the stakc
- If we see a `)` we check for `(` 
  - If we don't have any - We know that we are at a bad index
  - If we have some - We pop the index of `(`

At the end, we iterate through the remaining indexes in the stack and add all to a `list of indices to remove`

```java
public class MinRemoveToMakeValidParentheses {

  public static void main(String[] args) {
    System.out.println(new MinRemoveToMakeValidParentheses().minRemoveToMakeValid("lee(t(c)o)de)"));
  }

  public String minRemoveToMakeValid(String s) {
    // Using a stack
    Deque<Integer> stack = new ArrayDeque<>();
    Set<Integer> remove = new HashSet<>();

    StringBuilder sb = new StringBuilder();

    char[] charArray = s.toCharArray();
    for (int i = 0; i < charArray.length; i++) {
      char c = charArray[i];
      switch (c) {
        case '(':
          // Check if we already have an opening bracket
          stack.push(i);
          break;
        case ')':
          if (stack.isEmpty()) {
            remove.add(i);
          } else {
            stack.pop();
          }
          break;
      }
    }

    while (!stack.isEmpty()) {
      remove.add(stack.pop());
    }

    char[] array = s.toCharArray();
    for (int i = 0; i < array.length; i++) {
      if (!remove.contains(i)) {
        sb.append(array[i]);
      }
    }
    return sb.toString();
  }
}
```
