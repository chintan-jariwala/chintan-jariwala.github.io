---
title: Reconstruct Itinerary
date: 2023-09-10 00:00:00 +0000
categories: [Coding, netflix]
tags: [dfs, graph, eulerian-circuit]
author: jerry
---

## Problem
You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.
You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.



![itinerary1.png](/assets/img/itinerary1-graph.jpg)
### Example : 1
```textmate
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

![itinerary2.png](/assets/img/itinerary2-graph.jpg)
### Example: 2
```textmate
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

## Approach : 

This question can be a simple DFS traversal since we need to explore the itinerary

Traverse from One airport to the next and keep doing so until we cannot do anymore from the graph.

The twist here is - We need to return the result in `Smallest lexical order`

This means that for This question - `JFK - [SFO, ATL]` we must visit `JFK to ATL` first if we can.

Which means we need to sort the items in the destination queue for each source
Or 
We use a priority queue to fetch items properly


```java
public class Solution {
  public List<String> findItinerary(List<List<String>> tickets) {
    // base condition checks
    List<String> res = new ArrayList<>();
    if (tickets == null || tickets.size() == 0) {
      return res;
    }

    // Let's make a graph of all the places you can go
    // Since we need to traverse on all the values of a key
    // For ex JFK - [SFO, ATL] : We need to go to SFO and ATL
    // Lets create a Queueu in the map
    Map<String, PriorityQueue<String>> graph = new HashMap<>();

    for (List<String> ticket : tickets) {
      // There are 2 elements here guaranteed
      String source = ticket.get(0);
      String destination = ticket.get(1);

      // We create a graph
      graph.computeIfAbsent(source, val -> new PriorityQueue<>()).add(destination);
    }

    // Now we know we are supposed to start with JFK

    String start = "JFK";

    dfs(graph, start, res);

    return res;
  }

  private void dfs(Map<String, PriorityQueue<String>> graph, String start, List<String> res) {
    // DFS logic
    Queue<String> destinations = graph.get(start);
    while (destinations != null && !destinations.isEmpty()) {
      String next = destinations.poll();
      dfs(graph, next, res);
    }
    // at this point we explored all the things from the current
    res.add(0, start);
  }
}
```
