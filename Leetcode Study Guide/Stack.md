# Stack

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Use a stack when the most recent unresolved item should be handled first.
- Store indices instead of values when distances or positions matter.
- Use monotonic stacks for next greater, next smaller, and span problems.
- Pop while the top no longer satisfies the stack invariant.

## When To Use

Use a stack for nested structures, backtracking, valid parentheses, monotonic comparisons, and undo-like processing.

## Data Structure Implementation

```python
stack = []
stack.append(value)
top = stack[-1]
popped = stack.pop()
```

## Leetcode Style Code

Daily Temperatures: For each day, return how many days you must wait until a warmer temperature appears.

```python
def dailyTemperatures(self, temperatures):
    answer = [0] * len(temperatures)
    stack = []

    for i, temp in enumerate(temperatures):
        while stack and temperatures[stack[-1]] < temp:
            prev = stack.pop()
            answer[prev] = i - prev
        stack.append(i)

    return answer
```

### Valid Parentheses

Valid Parentheses: Given a string containing brackets, return whether every opening bracket is closed in the correct order.

```python
def isValid(self, s):
    stack = []
    pairs = {
        ")": "(",
        "]": "[",
        "}": "{",
    }

    for char in s:
        if char in pairs:
            if not stack or stack.pop() != pairs[char]:
                return False
        else:
            stack.append(char)

    return not stack
```
