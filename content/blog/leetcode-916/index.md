---
title: LeetCode 916. Word Subsets
date: "2022-07-29T21:17:03.284Z"
description: "Solution to LeetCode 916. Word Subsets"
---

[Word Subsets](https://leetcode.com/problems/word-subsets/)

>You are given two string arrays ```words1``` and ```words2```.
>
>A string ```b``` is a **subset** of string ```a``` if every letter in ```b``` occurs in ```a``` including multiplicity. For example, ```"wrr"``` is a subset of ```"warrior"``` but is not a subset of ```"world"```.
>
>A string ```a``` from ```words1``` is **universal** if for every string ```b``` in ```words2```, ```b``` is a subset of ```a```.
>
>Return an array of all the **universal** strings in ```words1```. You may return the answer in **any order**.

This problem wants us to return all strings in ```words1``` where every string in ```words2``` is a substring of $words1[i]$. We can reduce some of the time complexity by combining all strings in ```words2``` into a single string with the maximum number of each character in all the strings in ```words2```. For example, ```"wrr"``` and ```"ro"``` can become ```"wrro"```. From here we can use an array of character frequency counters to do the subset comparison, similar to how we solved [Group Anagrams](/leetcode-49).

```
public IList<string> WordSubsets(string[] words1, string[] words2) 
{
    List<string> result = new List<string>();
    
    int[] sFreq = CreateFrequencyArray(words2[0]);
    for(int i = 1; i < words2.Length; i++){
        int[] other = CreateFrequencyArray(words2[i]);
        
        for(int j = 0; j < 26; j++){
            sFreq[j] = Math.Max(sFreq[j],other[j]);
        }
    }
    
    for(int i = 0; i < words1.Length; i++){
        int[] freq = CreateFrequencyArray(words1[i]);
        
        bool isUniversal = true;
        
        for(int j = 0; j < 26; j++){
            if(freq[j] < sFreq[j]){
                isUniversal = false;
                break;
            }
        }
        
        if(isUniversal){
            result.Add(words1[i]);
        }
    }
    return result;
}

public int[] CreateFrequencyArray(string word)
{
    int[] freq = new int[26];
    
    for(int i = 0; i < 26; i++){
        freq[i] = 0;
    }
    
    foreach(char c in word){
        freq[c - 'a']++;
    }
    
    return freq;
}
```

The time complexity is $O(A+B)$. The space complexity is $O(A.Length + B.Length)$.