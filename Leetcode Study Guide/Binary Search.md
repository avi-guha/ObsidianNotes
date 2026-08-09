# Binary Search

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Define the search space and the condition that splits false from true.
- Use `while left <= right` for exact search.
- Use `while left < right` for lower-bound style search.
- Move bounds so the loop always makes progress.

## When To Use

Use binary search when the data is sorted or when the answer has a monotonic yes/no condition.

## Data Structure Implementation

```python
left = 0
right = len(nums) - 1

while left <= right:
    mid = (left + right) // 2
    if nums[mid] < target:
        left = mid + 1
    elif nums[mid] > target:
        right = mid - 1
    else:
        return mid
```

## Leetcode Style Code

Search Insert Position: Given a sorted array and a target, return the index where the target is found or should be inserted.

```python
def searchInsert(self, nums, target):
    left = 0
    right = len(nums)

    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid

    return left
```
