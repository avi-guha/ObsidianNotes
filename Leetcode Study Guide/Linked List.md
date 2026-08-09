# Linked List

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Use a dummy node when modifying the head is possible.
- Use slow and fast pointers for cycles and middle nodes.
- Reverse links by storing the next node before overwriting `next`.
- Draw pointer changes before coding deletion, insertion, or reversal.

## When To Use

Use linked list techniques when nodes must be rearranged by pointer changes, not by array indexing.

## Data Structure Implementation

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

## Leetcode Style Code

Reverse Linked List: Given the head of a singly linked list, reverse the list and return the new head.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(self, head):
    prev = None
    current = head

    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node

    return prev
```
