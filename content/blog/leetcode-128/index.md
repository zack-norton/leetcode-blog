---
title: LeetCode 128. Longest Consecutive Sequence
date: "2022-07-31T13:20:03.284Z"
description: "Solution to LeetCode 128. Longest Consecutive Sequence"
---

[Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

> Given an unsorted array of integers ```nums```, return *the length of the longest consecutive elements sequence*.
>
> You must write an algorithm that runs in $O(n)$ time.

### HashSet

We can solve this problem in $O(n)$ time by copying all elements in nums into a HashSet to give us constant $O(1)$ lookup time. From here, we loop through every element in the HashSet and check if it is the start of a sequence. The start of a sequence is defined by any element $nums[i]$ where $nums[i] - 1$ does not exist in the set. If this case is true, we start a loop looking for the next element in the sequence.

```
while *hashSet* contains current + 1
    current = current + 1
    count = count + 1
```

After we have found the length of the entire sequence, we store its length if it is the longest and continue.

```
public int LongestConsecutive(int[] nums) 
{
    HashSet<int> numSet = new HashSet<int>();
    for(int i = 0; i < nums.Length; i++){
        numSet.Add(nums[i]);
    }
    
    int max = 0;
    int current = 0;
    int currentI = 0;
    
    foreach(int i in numSet){
        if(!numSet.Contains(i - 1)){
            //start of sequence
            current = 1;
            currentI = i;
            
            while(numSet.Contains(currentI + 1)){
                current++;
                currentI++;
            }
            
            max = Math.Max(current, max);                
            
        }
    }
    return max;
}
```

Our time complexity is $O(n)$ and our space complexity is $O(n)$.