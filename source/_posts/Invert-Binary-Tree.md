---
title: Invert-Binary-Tree
date: 2019-03-26 01:13:03
tags:
 - problem
 - python
---

# [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Description
Invert a binary tree.

---

## Example

Input:
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

Output:
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

<!--more-->

---

## Solution

### Observation

If a node is not null, then

- its left node need to point to its right node.
- Likewise, right node attach to its left node.

If a node is a null, simply return null

---

### Simple recursive DFS

```
If incoming node is null:
    return null

Otherwise,
inverted_left = recursively call left node
inverted_right = recursively call right node

left of node point to `inverted_right`
right of node point to `inverted_left`

return node
```

---

### An iterative method [[ref](https://leetcode.com/problems/invert-binary-tree/discuss/62707/Straightforward-DFS-recursive-iterative-BFS-solutions)]

Use stack instead of method calling.

```
If incoming root is null:
return null

Otherwise,
stack = new Stack(root)

while stack is not empty:
    node = stack.top(), stack.pop()
    left = node.left
    node.left = node.right
    node.right = left
    
    if node.left is not null:
        stack.push(node.left)
    if node.right is not null:
        stack.push(node.right)

return root
```

---

### Run

Tree:
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
Init
```
stack = [TreeNode(4)]
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
6   9 1   3
```
Pop from stack, invert it.
Push left and right node.
```
stack = [TreeNode(7), TreeNode(2)]
node = TreeNode(4)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
6   9 3   1
```
Pop from stack, invert it.
Push left and right node.
```
stack = [TreeNode(7), TreeNode(3), TreeNode(1)]
node = TreeNode(2)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
6   9 3   1
```
Pop from stack, invert it.
```
stack = [TreeNode(7), TreeNode(3)]
node = TreeNode(1)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
6   9 3   1
```
Pop from stack, invert it.
```
stack = [TreeNode(7)]
node = TreeNode(3)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
Pop from stack, invert it.
Push left and right node.
```
stack = [TreeNode(9), TreeNode(6)]
node = TreeNode(7)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
Pop from stack, invert it.
```
stack = [TreeNode(9)]
node = TreeNode(6)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
Pop from stack, invert it.
```
stack = []
node = TreeNode(9)
```

---

Tree:
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

Finish
```
stack = []
```

## Code

### Recursive

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def invertTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if root is None:
            return None
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        root.left = right
        root.right = left
        return root
```

### Iterative

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root is None:
            return None
        
        stack = [root]
        
        while stack:
            node = stack.pop()
            
            left = node.left
            node.left = node.right
            node.right = left
            
            if node.left is not None:
                stack.append(node.left)
            
            if node.right is not None:
                stack.append(node.right)
        
        return root
        
```