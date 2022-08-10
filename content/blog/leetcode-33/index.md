---
title: LeetCode 33. Search in Rotated Sorted Array
date: "2022-08-10T00:08:03.284Z"
description: "Solution to LeetCode 33. Search in Rotated Sorted Array"
---

[Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

> There is an integer array ```nums``` sorted in ascending order (with **distinct** values).
>
> Prior to being passed to your function, ```nums``` is **possibly rotated** at an unknown pivot index ```k``` (```1 <= k < nums.length```) such that the resulting array is ```[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]``` (**0-indexed**). For example, ```[0,1,2,4,5,6,7]``` might be rotated at pivot index ```3``` and become ```[4,5,6,7,0,1,2]```.
>
> Given the array ```nums``` **after** the possible rotation and an integer ```target```, return *the index of* ```target``` *if it is in* ```nums```, *or* ```-1``` *if it is not in* ```nums```.
>
> You must write an algorithm with $O(log n)$ runtime complexity.

### Binary Search

We know that the array was sorted prior to the rotation, so we can apply binary search with some small modifications to account for the rotation to solve the problem. This will also fit the time complexity limitation of $O(logn)$.

Because the array has possibly been rotated, we need to account for that as we divide in in our binary search algorithm. We can call the smallest element in the array the rotation index, and before we split the array we should determine if either half contains the rotation index. If a particular half of the array does not contain the rotation index, we can check to see if that half contains our target value. If it does, we search that half. If it does not contain the target, we search the other half. Our resulting algorithm could look something like this:

```
while left < right
    mid = (left + right) / 2
    if array[mid] == target
        return mid
    if array[left] < array[mid]
        //first half not rotated
        if array[left] <= target < array[mid]
            //target in first half, search there
            right = mid - 1
        else
            //target not in first half, search second half
            left = mid + 1
    else
        //second half not rotated
        if array[mid] < target <= array[right]
            //target in second half, search here
            left = mid + 1
        else
            //target not in second half, search first half
            right = mid - 1
```

Using the above algorithm, we can easily implement a solution.

#### C#
```csharp
public int Search(int[] nums, int target) {
    if(nums.Length == 1){
        return nums[0] == target ? 0 : -1;
    }
    
    int start = 0;
    int end = nums.Length -1;
    
    while(start <= end){
        if(nums[start] == target) {
            return start;
        }
        if(nums[end] == target) {
            return end;
        }
        
        int mid = start + (end - start) / 2;
        if(nums[mid] == target){
            return mid;
        }
        if(nums[mid] >= nums[start]){
            if(target >= nums[start] && target < nums[mid]){
                end = mid - 1;
            }
            else{
                start = mid + 1;
            }
        }
        else{
            if(target <= nums[end] && target > nums[mid]){
                start = mid + 1;
            }
            else{
                end = mid - 1;
            }
        }
    }
    
    return -1;
}
```

#### Rust
```rust
pub fn search(nums: Vec<i32>, target: i32) -> i32 {
    if nums.len() == 1 {
        if nums[0] == target {
            return 0;
        }
        else{
            return -1;
        }
    }
    
    let mut start: usize = 0;
    let mut end = nums.len() - 1;
    
    while start <= end {
        if nums[start] == target {
            return start as i32;
        }
        if nums[end] == target {
            return end as i32;
        }
        
        let mid = start + (end - start) / 2;
        
        if nums[mid] == target {
            return mid as i32;
        }
        if nums[start] <= nums[mid] {
            if target >= nums[start] && target < nums[mid] {
                end = mid - 1;
            }
            else {
                start = mid + 1;
            }
        }
        else {
            if target > nums[mid] && target <= nums[end] {
                start = mid + 1;
            }
            else {
                end = mid - 1;
            }
        }
    }
    
    return -1;
}
```

The time complexity of the above solution is $O(logn)$ because we are able to divide the array down to a single element. The space complexity is $O(1)$ because the extra memory used is constant.