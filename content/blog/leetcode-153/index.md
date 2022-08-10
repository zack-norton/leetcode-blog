---
title: LeetCode 153. Find Minimum in Rotated Sorted Array
date: "2022-08-10T10:15:03.284Z"
description: "Solution to LeetCode 153. Find Minimum in Rotated Sorted Array"
---

[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

> Suppose an array of length ```n``` sorted in ascending order is **rotated** between ```1``` and ```n``` times. For example, the array ```nums = [0,1,2,4,5,6,7]``` might become:
>
> * ```[4,5,6,7,0,1,2]``` if it was rotated ```4``` times.
> * ```[0,1,2,4,5,6,7]``` if it was rotated ```7``` times.
>
> Notice that **rotating** an array ```[a[0], a[1], a[2], ..., a[n-1]]``` 1 time results in the array ```[a[n-1], a[0], a[1], a[2], ..., a[n-2]]```.
>
> Given the sorted rotated array ```nums``` of **unique** elements, return *the minimum element of this array*.
>
> You must write an algorithm that runs in $O(log n)$ time.

This problem is very similar to [Leetcode 33](/leetcode-33), but instead of looking for a specific value, we are looking for the minimum value in the array. Because the array was sorted before being rotated, we know that the minimum number in the array is any number $array[i] > array[j]$ where $j = i + 1$. Using this formula, we can apply a modified binary search to create an algorithm.

```
while start < end
    mid = (start + end) / 2

    if array[mid] > array[mid + 1]
        return array[mid + 1]

    if array[mid - 1] > array[mid] 
        return array[mid]

    if array[start] < array[mid]
        //first half is sorted, rotation index is in second half
        start = mid + 1
    else
        //second half is sorted, rotation index is in first half
        end = mid - 1
```

Using the above algorithm, we can come up with a solution.

#### C#
```csharp
public int FindMin(int[] nums) {
    if(nums.Length == 1){
        return nums[0];
    }
    
    int start = 0;
    int end = nums.Length - 1;
    
    if(nums[start] < nums[end]){
        return nums[0];
    }
    
    while(start <= end){
        int mid = (start + end) / 2;
        
        if(nums[mid] > nums[mid + 1]){
            return nums[mid+1];
        }
        if(mid > 0 && nums[mid - 1] > nums[mid]){
            return nums[mid];
        }
        
        if(nums[mid] > nums[start]){
            start = mid + 1;
        }
        else{
            end = mid - 1;
        }
    }
    
    return 0;
}
```

#### Rust
```rust
pub fn find_min(nums: Vec<i32>) -> i32 {
    if nums.len() == 1 {
        return nums[0];
    }
    
    let mut start: usize = 0;
    let mut end = nums.len() - 1;
    
    if nums[start] < nums[end] {
        return nums[start];
    }
    
    while start <= end {
        let mid = (start + end) / 2;
        
        if nums[mid] > nums[mid + 1] {
            return nums[mid+1];
        }
        if nums[mid - 1] > nums[mid] {
            return nums[mid];
        }
        
        if nums[start] < nums[mid] {
            start = mid + 1;
        }
        else{
            end = mid - 1;
        }
    }
    
    return 0;
}
```

The time complexity for this solution is $O(logn)$ since we are using binary search. The space complexity is $O(1)$.
