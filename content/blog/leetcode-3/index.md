---
title: LeetCode 3. Longest Substring Without Repeating Characters
date: "2022-07-26T14:03:03.284Z"
description: "Solution to LeetCode 3. Longest Substring Without Repeating Characters"
---

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

>Given a string ```s```, find the length of the **longest substring** without repeating characters.

### Sliding Window

A sliding window type algorithm fits really well with this problem. If we store seen characters and their most recent index in a HashMap, we can simply loop through each character in the array. If the current character is in our hashmap, we set our left pointer to the index of the last occurence of the current character. Otherwise, we set the max substring length seen to the current distance between the left and right pointers.

```
i = 0
For j = 0 to n
    if s[j] in HashMap
        i = index of last s[j]
    max = max of j - i + 1
    add s[j] to HashMap
```

Using the above algorithm, we can solve the problem.

#### C#
```csharp
    public int LengthOfLongestSubstring(string s) 
    {
        if(s.Length < 2){
            return s.Length;
        }
        
        int n = s.Length;
        int max = 0;
        Dictionary<char,int> seen = new Dictionary<char,int>();
        for(int i = 0, j = 0; j < n; j++){
            if(seen.ContainsKey(s[j])){
                i = Math.Max(seen[s[j]], i);
            }
            max = Math.Max(max, j - i + 1);
            if(seen.ContainsKey(s[j])){
                seen[s[j]] = j+1;
            }
            else{
                seen.Add(s[j], j+1);
            }
        }
        
        return max;
    }
```

#### Rust
```rust
pub fn length_of_longest_substring(s: String) -> i32 {
    use std::cmp;
    use std::collections::HashMap;
    
    let mut hash = HashMap::new();
    let mut max = 0;
    let mut start = 0;
    let mut end = 0;

    for (i, item) in s.chars().enumerate() {
        if let Some(j) = hash.get(&item) {
            if *j >= start {
                max = cmp::max(max, end - start);
                //move window
                start = *j + 1;
            }
        }
        end += 1;
        hash.insert(item, i);
    }
    max = cmp::max(max, end - start);
    max as i32
}
```

The time complexity for this solution is $O(n)$, since we simply need to loop through the string looking at each character. The space complexity is $O(m)$, where $m$ is the number of unique characters in the string.