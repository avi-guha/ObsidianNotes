# Queue

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Process items in first-in, first-out order.
- For BFS, push neighbors and mark them visited before enqueueing.
- Track level size when distance or depth matters.
- Use `deque`, not `list.pop(0)`, for efficient queue operations.

## When To Use

Use queues for BFS, shortest path in unweighted graphs, level order traversal, and simulations where order of arrival matters.

## Data Structure Implementation

```python
from collections import deque

queue = deque([start])
queue.append(next_value)
current = queue.popleft()
```

## Leetcode Style Code

Rotting Oranges: Given a grid of fresh and rotten oranges, return the minutes needed for all fresh oranges to rot, or `-1` if impossible.

```python
from collections import deque

def orangesRotting(self, grid):
    rows = len(grid)
    cols = len(grid[0])
    queue = deque()
    fresh = 0

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh += 1

    minutes = 0
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    while queue:
        r, c, minutes = queue.popleft()
        for dr, dc in directions:
            nr = r + dr
            nc = c + dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc, minutes + 1))

    return minutes if fresh == 0 else -1
```
