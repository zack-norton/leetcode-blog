---
title: LeetCode 49. Group Anagrams
date: "2022-07-27T22:27:03.284Z"
description: "Solution to LeetCode 49. Group Anagrams"
---

[Group Anagrams](https://leetcode.com/problems/group-anagrams/)

>Given an array of strings ```strs```, group **the anagrams** together. You can return the answer in **any order**.
>
>An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

### Sorting

If we sort the characters of each string, that sorted string can become a key in a HashMap. If two keys match, we add the unsorted string to a list of values in the HashMap for that key. The resulting set of values in the HashMap are the grouped anagrams.

```
public IList<IList<string>> GroupAnagrams(string[] strs) 
{
    Dictionary<string, IList<string>> sortedStrings = new Dictionary<string, IList<string>>();
       
    for(int i = 0; i < strs.Length; i++){
        char[] sortedChars = strs[i].ToCharArray();
        Array.Sort(sortedChars);
        string sorted = new string(sortedChars);
        if(sortedStrings.ContainsKey(sorted)){
            sortedStrings[sorted].Add(strs[i]);
        }
        else{
            sortedStrings.Add(sorted, new List<string>() { strs[i] });
        }
    }
        
    return sortedStrings.Values.ToList(); 
}
```

Since we are sorting each string in the list of strings, the time complexity is $O(n*klogk)$ where ```n``` is the number of strings in the list of strings and ```k``` is the number of characters in each string. The space complexity is $O(nk)$.

### Frequency Counter

What if instead of sorting the strings to create a key, we count the frequency of each character in the string and use that as the key? For each string, we create an array of length 26. We then loop through each character of the string and increase the count at the appropriate index in the array for the character. We then convert the array into a comma separated string and use that as the key for our HashMap.

```
public IList<IList<string>> GroupAnagrams(string[] strs) 
{
    Dictionary<string, IList<string>> groupedStrings = new Dictionary<string, IList<string>>();
        
    for(int i = 0; i < strs.Length; i++){
        int[] freq = new int[26];
        for(int j = 0; j < 26; j++){
            freq[j] = 0;
        }
            
        foreach(char c in strs[i]){
            freq[c - 'a']++;
        }
            
        string key = "";
        for(int j = 0; j < 26; j++){
            key += freq[j].ToString();
            key += ",";
        }
            
        if(groupedStrings.ContainsKey(key)){
            groupedStrings[key].Add(strs[i]);
        }
        else{
            groupedStrings.Add(key, new List<string>() { strs[i] });
        }
    }
        
    return groupedStrings.Values.ToList(); 
}
```

Since we are no longer sorting each string, the time complexity is $O(nk)$. The space complexity is still $O(nk)$.