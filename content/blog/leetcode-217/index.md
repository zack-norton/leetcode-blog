---
title: LeetCode 217. Contains Duplicate
date: "2022-07-27T16:24:03.284Z"
description: "Solution to LeetCode 217. Contains Duplicate"
---

[Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

>Given an integer array ```nums```, return ```true``` if any value appears **at least twice** in the array, and return ```false``` if every element is distinct.

This problem is trivialized by the use of a data structure with constant ($O(1)$) lookup time. We can use C#'s HashSet data structure.

```
    public bool ContainsDuplicate(int[] nums) 
    {
        HashSet<int> seenNums = new HashSet<int>();
        
        for(int i = 0; i < nums.Length; i++){
            if(!seenNums.Add(nums[i])){
                return true;
            }
        }
        
        return false;
    }
```

The time complexity for our solution is $O(n)$, since in the worst case (i.e. no duplicates), we must look at every element in the ```nums``` array. The space complexity is also $O(n)$, since we are copying every element to a new data structure.