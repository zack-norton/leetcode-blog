---
title: LeetCode 125. Valid Palindrome
date: "2022-07-31T13:47:03.284Z"
description: "Solution to LeetCode 125. Valid Palindrome"
---

[Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

> A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.
>
> Given a string ```s```, return ```true``` if it is a **palindrome**, or ```false``` otherwise.

### Regex + Two-pointers

Regex makes cleaning up the input string very simple. We can use a *replace* function to eliminate any non-alphanumeric character. 

```
s = Regex.Replace(s, "[^a-zA-Z0-9]", string.Empty).ToLower();
```

We determine if a string is a palindrome by comparing each character ```s[i]``` with ```s[i-n]```. If they are equal for every index ```i```, then the string is a palindrome.

```
public bool IsPalindrome(string s) 
{
    s = Regex.Replace(s, "[^a-zA-Z0-9]", string.Empty).ToLower();
    int left = 0;
    int right = s.Length - 1;
    
    while(left < right){
        if(s[left] != s[right]){
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

Our time complexity is $O(n)$ and our space complexity is $O(1)$.