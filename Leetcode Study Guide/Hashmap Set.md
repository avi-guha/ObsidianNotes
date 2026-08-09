# Hashmap / Set

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Use a set for fast membership checks.
- Use a hashmap to map a value to an index, count, or computed state.
- Convert nested lookup loops into one pass when possible.
- Be explicit about whether duplicate values matter.

## When To Use

Use hashmap or set patterns when the problem asks for existence, frequency, uniqueness, grouping, or constant-time lookup.

## Data Structure Implementation

```python
seen = set()
counts = {}

for value in nums:
    seen.add(value)
    counts[value] = counts.get(value, 0) + 1
```

## Leetcode Style Code

Two Sum: Given an array and a target, return the indices of two numbers that add up to the target.

```python
def twoSum(self, nums, target):
    index_by_value = {}

    for i, num in enumerate(nums):
        need = target - num
        if need in index_by_value:
            return [index_by_value[need], i]
        index_by_value[num] = i

    return []
```
