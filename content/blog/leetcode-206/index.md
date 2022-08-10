---
title: LeetCode 206. Reverse Linked List
date: "2022-08-10T13:53:03.284Z"
description: "Solution to LeetCode 206. Reverse Linked List"
---

[Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

> Given the ```head``` of a singly linked list, reverse the list, and *return the reversed list*.

Reversing a linked list can be done both iteratively and recursively. We will show solutions for both.

### Iterative

In the iterative solution to the problem, we need to scan through the list, keeping track of the previous element in the list and pointing the current node to it. 

#### C#

```csharp
public ListNode ReverseList(ListNode head) {
    ListNode previous = null;
    ListNode current = head;
    
    while(current != null){
        ListNode nextTemp = current.next;
        current.next = previous;
        previous = current;
        current = nextTemp;
    }
    
    return previous;
}
```

The time complexity for this solution is $O(n)$, because we must loop through every element in the list. The space complexity is $O(1)$.

### Recursive

The recursive solution requires us to scan all the way to the end of the list, and then setting each nodes next to its parent as we exit the recursion.

#### C#

```csharp
public ListNode ReverseList(ListNode head) {
    if(head == null || head.next == null){
        return head;
    }
    
    ListNode parent = ReverseList(head.next);
    
    head.next.next = head;
    
    head.next = null;
    
    return parent;
}
```

The time complexity for this solution is $O(n)$ like the iterative solution, but the space complexity is $O(n)$ because of the recursive calls.