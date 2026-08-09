# Array / String

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Track indices carefully and define what each index means.
- Prefer one pass when the answer depends on local state or a running best value.
- Sort first when order does not matter and comparison or grouping becomes easier.
- For strings, count characters with fixed-size arrays when the alphabet is small.

## When To Use

Use array and string techniques when the input is ordered, indexable, or can be scanned left to right while maintaining a small amount of state.

## Data Structure Implementation

```python
arr = [3, 1, 4]
arr.append(2)
arr.sort()

s = "leetcode"
chars = list(s)
chars[0] = "L"
updated = "".join(chars)
```

## Leetcode Style Code

Maximum Subarray: Given an integer array, return the largest possible sum of a contiguous subarray.

```python
def maxSubArray(self, nums):
    best = nums[0]
    current = nums[0]

    for num in nums[1:]:
        current = max(num, current + num)
        best = max(best, current)

    return best
```
