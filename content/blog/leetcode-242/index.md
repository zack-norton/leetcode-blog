---
title: LeetCode 242. Valid Anagram
date: "2022-07-27T16:47:03.284Z"
description: "Solution to LeetCode 242. Valid Anagram"
---

[Valid Anagram](https://leetcode.com/problems/valid-anagram/)

>Given two strings ```s``` and ```t```, return ```true``` if ```t``` is an anagram of ```s```, and ```false``` otherwise.
>
>An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

Two strings are anagrams of each other if they are the same length and contain the same characters. The length is straightforward to verify, just compare the number of characters in each string. Comparing the content of the two strings is where things could get slightly complicated. We could create two HashMaps that contain the unique characters of each string along with a count of the number of times that character occurs. We could also just sort the two strings and compare the result. Is there a simpler way?

### Frequency Counter

What if we create an array of length 26 to count the frequency of each character in the string? The problem constraints state the both strings consist of lowercase english characters, so there could be at most 26 unique characters in each string. After creating the frequency array, we can loop through the first string, increasing the count in the array for the appropriate character as they are seen. We then loop through the second string, decreasing the count at the appropriate index in the array for each character seen. In this second loop, if the frequency index for the current character is ever less than 0, we immediately know that the two strings are not anagrams.

We don't need to check that all frequencies equal zero after the second loop because we have already confirmed that both strings are the same length. We only care if a frequency is ever less than zero, because that indicates that somewhere in the strings there is a character mismatch.

```
    public bool IsAnagram(string s, string t) 
    {
        if(s.Length != t.Length){
            return false;
        }
        
        int[] charCount = new int[26];
        for(int i = 0; i < 26; i++){
            charCount[i] = 0;
        }
        
        for(int i = 0; i < s.Length; i++){
            charCount[s[i] - 'a']++;
        }
        
        for(int i = 0; i < t.Length; i++){
            charCount[t[i] - 'a'] --;
            if(charCount[t[i] - 'a'] < 0){
                return false;
            }
        }
        
        return true;
    }
```

Our frequency counter solution has a time complexity of $O(n+n)$, which is really just $O(n)$. Our space complexity is $O(26)$, which is really just constant time $O(1)$.