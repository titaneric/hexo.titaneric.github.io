---
title: Same-Tree
date: 2019-04-02 12:23:46
tags:
 - python
 - problem
---

# [100. Same Tree](https://leetcode.com/problems/same-tree/)

## Description
Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

---

## Example

**Example 1:**
```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```

---

**Example 2:**
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```

**Example 3:**
```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

---

<!--more-->

## Solution

Recursive or iterative method.
Both time complexity are $O(N)$.

Recursive:
```
If both node are null:
    return True
    
If one of node is null:
    return False

If value of p is not equal to value of q:
    return False
    
return recursively call left node of p and q
return recursively call right node of p and q
```

---

Iterative:
```
stack = [(p, q)]

while stack is not empty:
    p, q = stack.pop()
    if p and q is not the same:
        return False
    
    if p is not null:
        stack.push((p.left, q.left))
        stack.push((p.right, q.right))
        
return True
```

---

## Run

### Example 1

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Init

```
stack = [(TreeNode(1), TreeNode(1))]
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same, push their left and right node.

```
stack = [(TreeNode(2), TreeNode(2)), (TreeNode(3), TreeNode(3))]
p = TreeNode(1)
q = TreeNode(1)
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same, push their left and right node.

```
stack = [(TreeNode(2), TreeNode(2)), (null, null), (null, null)]
p = TreeNode(3)
q = TreeNode(3)
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same.
```
stack = [(TreeNode(2), TreeNode(2)), (null, null)]
p = null
q = null
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same.
```
stack = [(TreeNode(2), TreeNode(2))]
p = null
q = null
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same, push their left and right node.
```
stack = [(null, null), (null, null)]
p = TreeNode(2)
q = TreeNode(2)
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same.
```
stack = [(null, null)]
p = null
q = null
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is not empty, compare the top of the stack.
p and q are the same.
```
stack = []
p = null
q = null
```

---

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
```

Stack is empty, return True.
```
stack = []
```

---

### Example 2
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]
```

Init

```
stack = [(TreeNode(1), (TreeNode(1))]
```

---

```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]
```

Stack is not empty, compare the top of stack.
p and q are the same, push their left and right node.


```
stack = [(TreeNode(2), null), (null, TreeNode(2))]
p = TreeNode(1)
q = TreeNode(1)
```

---

```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]
```

Stack is not empty, compare the top of stack.
p and q are not the same, return False


```
stack = [(TreeNode(2), null)]
p = null
q = TreeNode(2)
```

## Code

### Iterative
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSame(self, p, q):
        if p is None and q is None:
            return True
        if p is None or q is None:
            return False
        if p.val != q.val:
            return False
        return True
    
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        stack = [(p, q)]
        while stack:
            p, q = stack.pop()
            if not self.isSame(p, q):
                return False
            if p:
                stack.append((p.left, q.left))
                stack.append((p.right, q.right))
        return True
```

### Recursive

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if p is None and q is None:
            return True
        if p is None or q is None:
            return False
        return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```