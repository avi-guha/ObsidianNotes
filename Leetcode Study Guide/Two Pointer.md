# Two Pointer

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Place one pointer at each end when the array is sorted or the answer depends on pair sums.
- Move the pointer that can still improve the result.
- Use slow and fast pointers to overwrite, compact, or detect cycles.
- Maintain a clear loop invariant before moving either pointer.

## When To Use

Use two pointers when you need to compare or shrink from both ends, remove duplicates in place, merge sorted inputs, or scan with different speeds.

## Data Structure Implementation

```python
left = 0
right = len(nums) - 1

while left < right:
    total = nums[left] + nums[right]
    if total < target:
        left += 1
    else:
        right -= 1
```

## Leetcode Style Code

Two Sum II: Given a sorted array, return the 1-indexed positions of two numbers that add up to the target.

```python
def twoSum(self, numbers, target):
    left = 0
    right = len(numbers) - 1

    while left < right:
        total = numbers[left] + numbers[right]
        if total == target:
            return [left + 1, right + 1]
        if total < target:
            left += 1
        else:
            right -= 1

    return []
```
