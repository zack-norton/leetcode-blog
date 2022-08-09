---
title: LeetCode 11. Container With Most Water
date: "2022-07-31T14:27:03.284Z"
description: "Solution to LeetCode 11. Container With Most Water"
---

[Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

> You are given an integer array ```height``` of length ```n```. There are ```n``` vertical lines drawn such that the two endpoints of the $i^{th}$ line are ```(i, 0)``` and ```(i, height[i])```.
>
> Find two lines that together with the x-axis form a container, such that the container contains the most water.
>
> Return *the maximum amount of water a container can store*.
>
> **Notice** that you may not slant the container.

### Two-pointers

 The amount of water that a container can hold is just the area between the two vertical lines. Since we cannot slant the container, the formula for the area is $min(height[i], height[j]) * (i - j)$. 

 The brute force approach requires us to calculate the area for all pairs of heights. We can instead solve this problem with two pointers starting at the beginning and end of the array. We calculate the area for the current pair of heights and compare it to the max. We then increment or decrement the appropriate pointer with the smallest height. The resulting algorithm would look like this:

 ```
while left < right
    max = max(max, min(heights[left], heights[right]) * (right - left))
    if heights[left] < heights[right]
        left++
    else
        right--

 ```

 Using the above algorithm, we can implement a solution.

#### C#
 ```csharp
 public int MaxArea(int[] height) {
    int max = 0;
    int left = 0;
    int right = height.Length - 1;
    
    while(left < right){
        max = Math.Max(max, (Math.Min(height[left], height[right]) * (right - left)));
        if(height[left] < height[right]){
            left++;
        }
        else{
            right--;
        }            
    }
    
    return max;
}
 ```

 #### Rust
 ```Rust
pub fn max_area(height: Vec<i32>) -> i32 {
    use std::cmp;
    
    let mut max = 0;
    let mut left: usize = 0;
    let mut right: usize = height.len() - 1;
    
    while left < right {
        max = cmp::max(max, (cmp::min(height[left], height[right]) * (right as i32 - left as i32)));
        if height[left] < height[right] {
            left += 1;
        }
        else {
            right -= 1;
        }
    }
    return max;
}
 ```

 The time complexity of our algorithm is $O(n)$ since we calculate the area between two points $n$ times. The space complexity is $O(1)$.