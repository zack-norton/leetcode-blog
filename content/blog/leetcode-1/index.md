---
title: LeetCode 1. Two Sum
date: "2022-07-25T23:26:03.284Z"
description: "Solution to LeetCode 1. Two Sum"
---

[Two Sum](https://leetcode.com/problems/two-sum/)


> Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. 
>
> You may assume that each input would have exactly one solution, and you may not use the same element twice.  
>
> You can return the answer in any order. 

This problem is asking us to determine whether a given array of integers contains two values that sum up to a given target value. i.e. 
```
array[i] + array[j] = target
i != j
```

### Brute Force

The brute force approach to this problem is fairly straightforward. We simply calculate the sum of each pair of integers in the array and compare them to the target value. 

```
    public int[] TwoSum(int[] nums, int target) 
    {    
        int[] result = new int[2];
        for(int i = 0; i < nums.Length - 1; i++){
            for(int j = i+1; j < nums.Length; j++){
                if(nums[i] + nums[j] == target){
                    result[0] = i;
                    result[1] = j;
                    return result;
                }
            }
        }
        
        return null;
    }
```

*Can you see the problem with this approach?*

The time complexity for the brute force approach to this problem is $O(n^2)$, since in the worst case we have to compare every pair of integers in the array. **Can we improve that?**

### HashMap

Can we complete the task in linear time? What if we store *seen* values in a data structure with constant lookup time as we scan through the array? As we reach a new value, we check our list of seen values to see if it contains the compliment of the current value. Here is an example that uses a HashMap as our data structure with constant lookup time:

```
    public int[] TwoSum(int[] nums, int target) 
    {
        Dictionary<int,int> seen = new Dictionary<int,int>();
        
        for(int i = 0; i < nums.Length; i++){
            if(seen.ContainsKey(target - nums[i])){
                return new int[] { seen[target - nums[i]], i };
            }
            if(!seen.ContainsKey(nums[i])){
                seen.Add(nums[i], i);
            }
        }
        
        return null;
    }
```

Since we only have to loop through the array one time, our time complexity is $O(n)$. However, since we are storing the seen values in a HashMap, our space complexity is $O(n)$.