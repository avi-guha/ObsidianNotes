# Binary Tree DFS: Recursive

[Back to Leetcode Study Guide](Leetcode%20Study%20Guide.md)

## Problem Solving Strategies

- Define what each recursive call returns.
- Handle the base case before reading child nodes.
- Use preorder for top-down state, inorder for BST ordering, and postorder for child summaries.
- Pass path state down and aggregate answers upward.

## When To Use

Use recursive DFS when tree depth is manageable and the solution naturally splits into left and right subtree results.

## Data Structure Implementation

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## Leetcode Style Code

Maximum Depth of Binary Tree: Given a binary tree, return the number of nodes on the longest path from root to leaf.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(self, root):
    if not root:
        return 0

    left_depth = self.maxDepth(root.left)
    right_depth = self.maxDepth(root.right)
    return 1 + max(left_depth, right_depth)
```
