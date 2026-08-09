# Binary Tree DFS: Iterative

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Use an explicit stack to avoid recursion.
- Store the node plus any state needed at that point, such as depth or path sum.
- Push right before left if you want preorder left-to-right processing.
- Use a visited marker for postorder-style iterative traversal.

## When To Use

Use iterative DFS when recursion depth may be large, when the problem asks for manual stack behavior, or when you want tighter control over traversal state.

## Data Structure Implementation

```python
stack = [(root, 1)]

while stack:
    node, depth = stack.pop()
    if node.right:
        stack.append((node.right, depth + 1))
    if node.left:
        stack.append((node.left, depth + 1))
```

## Leetcode Style Code

Path Sum: Given a binary tree and a target sum, return whether any root-to-leaf path adds up to the target.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def hasPathSum(self, root, targetSum):
    if not root:
        return False

    stack = [(root, root.val)]

    while stack:
        node, total = stack.pop()
        if not node.left and not node.right and total == targetSum:
            return True
        if node.right:
            stack.append((node.right, total + node.right.val))
        if node.left:
            stack.append((node.left, total + node.left.val))

    return False
```
