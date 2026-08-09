# Prefix Sum

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Convert range sum queries into subtraction between two stored sums.
- Store the earliest or most useful index for each prefix value.
- For subarray sum equals `k`, look for previous prefix sum `current - k`.
- Add a zero prefix before scanning to handle subarrays starting at index `0`.

## When To Use

Use prefix sums when repeated range sums, subarray sums, or cumulative differences are central to the problem.

## Data Structure Implementation

```python
prefix = [0]
for num in nums:
    prefix.append(prefix[-1] + num)

range_sum = prefix[right + 1] - prefix[left]
```

## Leetcode Style Code

Subarray Sum Equals K: Given an integer array, count how many contiguous subarrays sum to `k`.

```python
from collections import defaultdict

def subarraySum(self, nums, k):
    counts = defaultdict(int)
    counts[0] = 1
    current = 0
    total = 0

    for num in nums:
        current += num
        total += counts[current - k]
        counts[current] += 1

    return total
```
