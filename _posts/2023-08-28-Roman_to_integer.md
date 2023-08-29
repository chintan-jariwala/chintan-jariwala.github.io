---
title: Roman To Integer
date: 2023-08-28 00:00:00 +0000
categories: [Coding, Top150]
tags: [hashtable, math, string]
author: jerry
---

## Problem
Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

### Example : 1
```textmate
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just `two ones added` together. `12` is written as `XII`, which is simply `X` + `II`. The number `27` is written as `XXVII`, which is `XX` + `V` + `II`.


- `I` can be placed before `V` `(5)` and `X` `(10)` to make `4` and `9`.
- `X` can be placed before `L` `(50)` and `C` `(100)` to make `40` and `90`.
- `C` can be placed before `D` `(500)` and `M` `(1000)` to make `400` and `900`.


### Example : 2
```textmate
Input: s = "III"
Output: 3
Explanation: III = 3.
```
### Example : 3
```textmate
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Approach : 1 

The approach is simple simulation
- Only tricky part here is, we need to know two characters at the same time because,
  - `I` can come before `V` and it will change our total
  - So for Number `XXIV` -> If we, calculate normally
    - For each `X` we add `10`
    - For `I` we add `1` 
    - For `V` we add `5` then This will give a wrong answer
  - Instead if we track 2 numbers, 1 - Actual and 2 -Prev
    - For each `X` we get `10`
    - For `I` we add `1`
    - For `V` we will not add `5` because we know `IV` together is `4` but we already considered `I` by itself and added `1` so for `IV` we will add `3`



```java
public class Solution {

    public static void main(String[] args) {
        new Solution().romanToInt("LVIII");
    }
    
    public int romanToInt(String s) {
      
      // Basic validation
      if (s == null || s.isEmpty()) {
        return 0;
      }
      
      // Initially we won't have a prefix 
      char prefix = ' ';
      
      // store result
      int res = 0;
      
      for (char current : s.toCharArray()) {
        res += getVal(current, prefix);
        // Before we exit we update the prefix
        prefix = current;
      }
      return res;
    }
    
    private int getVal(char current, char prev) {
      // Multiple conditions here
      return switch (current) {
        case 'I' -> 1;
        case 'V' -> (prev == 'I') ? 3 : 5;
        case 'X' -> (prev == 'I') ? 8 : 10;
        case 'L' -> (prev == 'X') ? 30 : 50;
        case 'C' -> (prev == 'X') ? 80 : 100;
        case 'D' -> (prev == 'C') ? 300 : 500;
        case 'M' -> (prev == 'C') ? 800 : 1000;
        default -> 0;
      }
    }
    
    
}
```
