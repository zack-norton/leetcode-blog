---
title: LeetCode 20. Valid Parentheses
date: "2022-08-09T21:25:03.284Z"
description: "Solution to LeetCode 20. Valid Parentheses"
---

[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

> Given a string ```s``` containing just the characters ```'('```, ```')'```, ```'{'```, ```'}'```, ```'['``` and ```']'```, determine if the input string is valid.
>
> An input string is valid if:
>
> 1. Open brackets must be closed by the same type of brackets.  
> 2. Open brackets must be closed in the correct order.  

### Stack

Valid parentheses follows a LIFO pattern for evaluation. This means that given the string ```"([{"```, to begin closing the parentheses the next character must be ```'}'```. With this knowledge, we can determine that a Stack data structure can help us solve the problem. To evaluate the given string, we can loop through it checking each character. If the current character is ```'('```, ```'{'``` or ```'['```, we push the matching closing character onto the stack. If the current character is ```')'```, ```'}'``` or ```']'```, we pop from the stack and compare the popped character to the current character. If the popped character and current character match, we continue. If they do not match or if the stack was empty, we immediately return false indicating the parentheses were not valid. After looping through all the characters in the string, we check to see if the stack is empty. If it is empty, the parentheses are valid. Otherwise, they are not.

#### C#
```csharp
public bool IsValid(string s) {
    Stack<char> charStack = new Stack<char>();
    
    foreach(char c in s.ToCharArray()){
        if(c == '('){
            charStack.Push(')');
        }
        else if(c == '{'){
            charStack.Push('}');
        }
        else if(c == '['){
            charStack.Push(']');
        }
        else{
            if(charStack.Count() > 0 && charStack.Peek() != c || charStack.Count() == 0){
                return false;
            }
            charStack.Pop();
        }
    }
    
    return charStack.Count() == 0;
}
```

#### Rust
```rust
pub fn is_valid(s: String) -> bool {
    let mut stack: Vec<char> = Vec::with_capacity(s.len());
    
    for c in s.chars() {
        match c {
            '(' => stack.push(')'),
            '{' => stack.push('}'),
            '[' => stack.push(']'),
            _ => {
                if stack.pop() != Some(c) {
                    return false;
                }
            }
        }
    }
    
    
    return stack.is_empty();
}
```

The time complexity is $O(n)$, since we must look at every character in the string. The space complexity is $O(n)$, since in the worst case we must create an array of length $n$ for an invalid string of parentheses.