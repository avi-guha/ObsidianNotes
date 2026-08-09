# Heap / Priority Queue

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Use a min-heap for repeated access to the smallest item.
- Store negative values to simulate a max-heap.
- Keep heap size fixed for top `k` problems.
- Store tuples when priority and payload both matter.

## When To Use

Use heaps when you repeatedly need the next smallest or largest item, top `k`, merging sorted inputs, or scheduling by priority.

## Data Structure Implementation

```python
import heapq

heap = []
heapq.heappush(heap, (priority, value))
priority, value = heapq.heappop(heap)
```

## Leetcode Style Code

Kth Largest Element in an Array: Given an array, return the `k`th largest element without fully sorting the array.

```python
import heapq

def findKthLargest(self, nums, k):
    heap = []

    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)

    return heap[0]
```
