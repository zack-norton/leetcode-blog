---
title: LeetCode 271. Encode and Decode Strings
date: "2022-07-30T17:33:03.284Z"
description: "Solution to LeetCode 271. Encode and Decode Strings"
---

[Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)

> Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.
>
> Machine 1 (sender) has the function:
>
> ``` 
> string encode(vector<string> strs) {
>  // ... your code
>  return encoded_string;
> }
> ```
>
> Machine 2 (receiver) has the function:
> 
> ```
> vector<string> decode(string s) {
>   //... your code
>   return strs;
> }
> ```
>
> So Machine 1 does:
>
> ```
> string encoded_string = encode(strs);
> ```
>
> and Machine 2 does:
>
>```
> vector<string> strs2 = decode(encoded_string);
>```
>
> ```strs2``` in Machine 2 should be the same as ```strs``` in Machine 1.
>
> Implement the ```encode``` and ```decode``` methods.
>
> You are not allowed to solve the problem using any serialize methods (such as ```eval```).

This long problem statement essentially boils down to the task of creating a universal way to delimit a list of strings. The official leetcode solution wants us to use the length of each individual string written as a 4-byte integer as the delimiter. But we can come up with a simpler solution that just involves writing the length of the string and a specfic non-numeric delimiter character.

### Length + Character

When encoding the list of strings into a single string, we append the length of the current string onto the front of the string, followed by the delimiter character "|", with the resulting string looking like this:

```
s[i].length + "|" + s[i]
```

The fully encoded string will be:

```
s[0].length + "|" + s[0] + ... + s[i].length + "|" + s[i] + ... + s[n].length + "|" + s[n]
```

When decoding the string, we read until the first occurence of our delimiter character. The characters prior to the delimiter contain the length of the next string to be read. So we can read that number of characters of the delimiter to get our decoded string.

```
// Encodes a list of strings to a single string.
public string encode(IList<string> strs) 
{
    StringBuilder sb = new StringBuilder();
    
    foreach(string s in strs){
        sb.Append(s.Length.ToString());
        sb.Append("|");
        sb.Append(s);
    }
    
    return sb.ToString();
}

// Decodes a single string to a list of strings.
public IList<string> decode(string s) 
{
    int index = 0;
    List<string> result = new List<string>();
    int start = 0;
    int length = 0;
    
    while(index < s.Length){
        start = index;
        while(s[index] != '|'){
            index++;
        }
        length = int.Parse(s.Substring(start, index - start));
        index++;
        
        result.Add(s.Substring(index, length));
        index += length;
    }
    return result;
}
```

The time complexity for encoding and decoding is $O(n)$, and the space complexity is $O(n)$. 