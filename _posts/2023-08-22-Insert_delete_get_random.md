---
title: Insert Delete Get Random O(1)
date: 2023-08-22 00:00:00 +0000
categories: [Coding, Top150]
tags: [array, hash-table, math, design, randomized]
author: jerry
---

## Problem

Implement the **RandomizedSet** class:
- `RandomizedSet()` Initializes the RandomizedSet object.
- `bool insert(int val)` Inserts an item val into the set if not present. Returns `true` if the item was not present, `false` otherwise.
- `bool remove(int val)` Removes an item val from the set if present. Returns true if the item was present, false otherwise.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

### Example : 1
```textmate
Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
```

### Constraints:
- `-231 <= val <= 231 - 1`

## Approach : 1 

This problem is easy until duplicate values are not allowed.

Simply we 
- Create a List and add values to the list.
- Create a Map and add new values in that map too and we store locations of each values in the value Field where they were added in the list Above

Example 
```textmate
Add (5) --> Put List(5) (Since 5 is at index 0) -> Map(5, 0)
Add (2) --> Put List(5, 2) (Since 2 is at index 1) -> Map(5, 0)(2, 1)
...
Remove(5) --> Check if we have that -> Yes we do
-> Get the position of it in the list  = 0th Index
-> We take it and swap it with the last one
and remove the last one
```

```java
private List<Integer> list;
private Map<Integer, Integer> locations;

public RandomizedSet() {
    list = new ArrayList<>();
    locations = new HashMap<>();
}

public boolean insert(int val) {
    // Check if we already have the value
    boolean contains = locations.containsKey(val);
    // If we have it - No duplicates allowed
    if (contains)
        return false;
    // We can add the value
    list.add(val);
    locations.put(val, list.size() - 1);
    return true;
}

public boolean remove(int val) {
    // Check if we have the value to remove
    boolean contains = locations.containsKey(val);
    // Cannot remove something we don't have
    if (!contains) {
        return false;
    }
    // If we have it, We remove it.
    // we know the index of last element
    // it can be removed from the list in O(1)

    // Get the index of the element we need to remove
    int loc = locations.get(val);
    // Best case Scenario : that is the last element
    if (loc != list.size() - 1) {
        // we need to swap
        // Last element
        int lastElement = list.get(list.size() - 1);
        // Update the location of the last element with our current
        locations.put(lastElement, loc);
        // Update the element in the list
        list.set(loc, lastElement);
    }
    // Remove the val
    list.remove(list.size() - 1);
    locations.remove(val);
    return true;
}

public int getRandom() {
    return list.get(new Random().nextInt(list.size()));
}
```


## Problem : Lets allow duplicates

We do the same logic but we maintain a list of occurances of each element in a HashSet
> Why HashSet ?  Because we need to access their indexes in also O(1)

```java
class RandomizedCollection {

    private List<Integer> numbers;
    private Map<Integer, Set<Integer>> locations;
    private Random random;

    public RandomizedCollection() {
        numbers = new ArrayList<>();
        locations = new HashMap<>();
        random = new Random();
    }
    
    public boolean insert(int val) {
        // Check if we already have the value
        boolean contains = locations.containsKey(val);
        // If we don't have it, Create a new entry
        if (!contains)
            locations.put(val, new HashSet<>());
        // We can add the value
        numbers.add(val);
        // Also add the location in the map and in the list
        locations.get(val).add(numbers.size() - 1);
        return !contains;
    }

    public boolean remove(int val) {
        // Check if we have the value to remove
        boolean contains = locations.containsKey(val);
        // Cannot remove something we don't have
        if (!contains) {
            return false;
        }
        // If we have it, We remove it.
        // we know the index of last element
        // it can be removed from the list in O(1)

        // Get the index we need to change
        int indexToModify = locations.get(val).iterator().next();
        // Remove it from the set
        locations.get(val).remove(indexToModify);

        // If that is not the last item
        if (indexToModify < numbers.size() - 1) {
            // Swap is needed
            int lastItem = numbers.get(numbers.size() - 1);
            numbers.set(indexToModify, lastItem);
            locations.get(lastItem).remove(numbers.size() - 1);
            locations.get(lastItem).add(indexToModify);
        }
        // Remove the last
        numbers.remove(numbers.size() - 1);
        if (locations.get(val).isEmpty()) {
            locations.remove(val);
        }
        return true;
    }

    public int getRandom() {
        return numbers.get(random.nextInt(numbers.size()));
    }
}
```
