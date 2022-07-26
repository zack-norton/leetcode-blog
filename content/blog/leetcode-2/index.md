---
title: LeetCode 2. Add Two Numbers
date: "2022-07-26T13:18:03.284Z"
description: "Solution to LeetCode 2. Add Two Numbers"
---

[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

>You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
>
>You may assume the two numbers do not contain any leading zero, except the number 0 itself.

This problem is simply asking us to add two numbers and return the sum. The challenging part of the problem is that the numbers are represented as linked lists, with individual digits stored as nodes in the list. Luckily, the numbers are stored in reverse order, so we do not have to do any extra work to reverse the list to apply our solution.

```
  1 -> 2 -> 3
+ 5 -> 6 -> 7
= 6 -> 8 -> 0 -> 1

OR

321 + 765 = 1086
```

### Elementary Addition

When adding two numbers, a simple approach is to align the numbers vertically and sum the individual pairs of digits. If the summation results in a number that is larger than 9 (i.e. $6 + 8 = 14$), we write the value in the 1's place as the result and then carry the number in the 10's place over to the next summation, adding it there. Since the two numbers we are asked to sum are in reverse order, we can apply the same principal here. 

```
For every pair of numbers:
    sum = a.val + b.val + carry
    onesValue = sum % 10 //This gets the value that goes in the 1's place
    carry = sum / 10 //If sum > 9, carry will be 1
```

With that basic algorithm, we can sum each pair of digits until one of the lists is empty. After reaching the end of one of the lists, we can simply copy any remaining values from the other list into the result (remebering to carry the 1 if needed). The resulting function could look something like this:

```
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) 
    {
        ListNode dummy = new ListNode();
        ListNode iterator = dummy;
        int sum = 0;
        int newVal = 0;
        int carry = 0;
        
        while(l1 != null && l2 != null){
            sum = l1.val + l2.val + carry;
            newVal = sum % 10;
            carry = sum / 10;
            
            iterator.next = new ListNode(newVal);
            iterator = iterator.next;
            l1 = l1.next;
            l2 = l2.next;
        }
        
        while(l1 != null){
            sum = l1.val + carry;
            newVal = sum % 10;
            carry = sum / 10;
            
            iterator.next = new ListNode(newVal);
            iterator = iterator.next;
            l1 = l1.next;
        }
        
        while(l2 != null){
            sum = l2.val + carry;
            newVal = sum % 10;
            carry = sum / 10;
            
            iterator.next = new ListNode(newVal);
            iterator = iterator.next;
            l2 = l2.next;
        }
        
        if(carry > 0){
            iterator.next = new ListNode(carry);
        }
        
        return dummy.next;
    }
```

The resulting time complexity would be $O(Max(m,n))$ where $m$ and $n$ are the lengths of the two linked lists. We have to visit each node at least once, but not more, so it is linear time.