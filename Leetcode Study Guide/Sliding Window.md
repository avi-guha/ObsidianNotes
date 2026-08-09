# Sliding Window

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Expand the right edge to include new values.
- Shrink the left edge while the window is invalid or can be improved.
- Store only the state needed to evaluate the current window.
- Decide whether the window is fixed size or variable size before coding.

## When To Use

Use sliding window for contiguous subarrays or substrings where you need a maximum, minimum, count, or validity condition.

## Data Structure Implementation

```python
from collections import defaultdict

counts = defaultdict(int)
left = 0

for right, value in enumerate(nums):
    counts[value] += 1
    while not window_is_valid(counts):
        counts[nums[left]] -= 1
        left += 1
```

## Leetcode Style Code

Longest Substring Without Repeating Characters: Given a string, return the length of the longest substring with no repeated characters.

```python
def lengthOfLongestSubstring(self, s):
    seen = set()
    left = 0
    best = 0

    for right, char in enumerate(s):
        while char in seen:
            seen.remove(s[left])
            left += 1
        seen.add(char)
        best = max(best, right - left + 1)

    return best
```
