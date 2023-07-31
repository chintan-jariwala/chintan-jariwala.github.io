---
title: Text Justification
date: 2023-07-30 00:00:00 +0000
categories: [Coding, Top100]
tags: [array, string, simulation, trial-error, google, hard]
author: jerry
---

## Problem
Given an array of strings `words` and a width `maxWidth`, format the text such that each line has **exactly maxWidth characters** and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

Note:

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
- The input array `words` contains at least one word.

### Example 1:
```textmate
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```
### Example 2:
```textmate
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```
### Example 3
```textmate
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

### Approach : Simulation
This question deals with more of smaller nuances rather than the complexity. What we need to keep in mind are the constriants given in the question

Lets break the answer down in 2 primary methods
- First we figure out how many words can I add in any line given our starting word index.
- Once we know the range of words that can fit on the first line, we then deal with adding the spaces between those words evenly. If the spaces cannot be evenly distributed, then we add more spaces on the left than right

We keep doing this until we exhaust all the words.

The solution below has comments for easy understanding.
#### Solution

##### TC = `` (Not Sure)
##### SC = `` (Not Sure)

```java
  public List<String> fullJustify(String[] words, int maxWidth) {
      // We start from the first word on the left
      int left = 0;
  
      // Resulting list
      List<String> result = new ArrayList<>();
  
      // Prefix sum of all the words
      List<Integer> preSum = new ArrayList<>();
      preSum.add(0);
      for (int i = 1; i <= words.length; i++) {
          String word = words[i-1];
          preSum.add(preSum.get(i-1) + word.length());
      }
  
  
      // Go through all the words
      while (left < words.length) {
          // Find and add all the words you can fit in 1 line
          int right = findRight(left, words, maxWidth);
  
          // Justify the words with appropriate spaces between them
          result.add(justify(left, right, words, maxWidth, preSum));
  
          // Move on to the next word
          left = right + 1;
      }
  
      return result;
  }
  
  private int findRight(int left, String[] words, int maxWidth) {
      // We start adding the words from the left side
      int right = left;
  
      // First add the first word on the new line. THis word must fit so we add it by default
      int sum = words[right++].length();
  
      // We check for 2 conditions here
      // If the word is the last word we break else
      // We add that word and add 1 space after that word then check if that exceeds our width
      while (right < words.length && (sum + 1 + words[right].length()) <= maxWidth) {
          sum += 1 + words[right].length();
  
          // Go to the next word
          right++;
      }
      // At this point, the right pointer needs to come back since we broke form the loop above
      return right - 1;
  }
  
  private String justify(int left, int right, String[] words, int maxWidth, List<Integer> preSum) {
      // If we can only fit 1 word, or we are down to the last word in a new line
      if (right - left == 0) {
          return padResult(words[left], maxWidth);
      }
  
      // IF we are at the last line, we don't need to justify
      boolean isLastLine = (right == words.length - 1);
  
      // Mandatory spaces between words
      int spacedBetweenWords = right - left;
  
      // Total available spaces between words
      int totalAvailableSpaces = maxWidth - wordsLength(left, right, preSum);
  
      // If we are at the last word, we just append all empty spaces
      String space = isLastLine ? " " : blank(totalAvailableSpaces / spacedBetweenWords);
  
      // Additional spaces
      int remainder = isLastLine ? 0 : totalAvailableSpaces % spacedBetweenWords;
  
      StringBuilder sb = new StringBuilder();
      for (int i = left; i <= right; i++) {
          sb.append(words[i])
                  .append(space)
                  .append(remainder-- > 0 ? " " : "");
      }
      return padResult(sb.toString().trim(), maxWidth);
  }
  
    private int wordsLength(int left, int right, List<Integer> preSum) {
        return preSum.get(right+1) - preSum.get(left);
    }
    
    private String padResult(String word, int maxWidth) {
        System.out.println(maxWidth + " - " + word.length());
        return word + blank(maxWidth - word.length());
    }
    
    private String blank(int length) {
        return new String(new char[length]).replace('\0', ' ');
    }
```
