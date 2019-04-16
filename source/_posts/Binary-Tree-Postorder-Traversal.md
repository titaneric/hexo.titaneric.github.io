---
title: Binary-Tree-Postorder-Traversal
date: 2019-04-16 12:35:50
tags:
 - python
 - problem
---

# [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Description
Given a binary tree, return the postorder traversal of its nodes' values.

---

## Example
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

<!--more-->

---

## Key

---

Push reverse postorder traversal to list.

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```

Reverse postorder traversal

```
[1, 3, 7, 6, 2, 5, 4]
```

Preorder traversal

```
[1, 2, 4, 5, 3, 6, 7]
```

We visit right node before left node.
Thus, the visiting sequence is `root right left`.

---

- Like other iterative order traversal, use stack to memorize nodes.
- To ensure that the right node is visited before left node, we push left node **before** right node.

---

## Algorithm

```
1. Push root to first stack, init answer list
2. while first stack is not empty:
       node = pop from first stack
       if node is not None:
           Push node to answer list
           
       if node.left is not None:
           Push node.left to first stack
           
       if node.right is not None:
           Push node.right to first stack
3. Get reverse postorder traversal, return reversed answer list
```
[GeeksforGeeks-Iterative Postorder Traversal](https://www.geeksforgeeks.org/iterative-postorder-traversal/)

---

## Run

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Initialize
```python
stack = [TreeNode(1)]
ans = []
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push left and right node into stack.
```python
stack = [TreeNode(2), TreeNode(3)]
ans = [1]

node = TreeNode(1)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push left and right node into stack.
```python
stack = [TreeNode(2), TreeNode(6), TreeNode(7)]
ans = [1, 3]

node = TreeNode(3)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push node to ans
```python
stack = [TreeNode(2), TreeNode(6)]
ans = [1, 3, 7]

node = TreeNode(7)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push node to ans
```python
stack = [TreeNode(2)]
ans = [1, 3, 7, 6]

node = TreeNode(6)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push left and right node into stack.
```python
stack = [TreeNode(4), TreeNode(5)]
ans = [1, 3, 7, 6, 2]

node = TreeNode(2)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push node to ans
```python
stack = [TreeNode(4)]
ans = [1, 3, 7, 6, 2, 5]

node = TreeNode(5)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Push node to ans
```python
stack = []
ans = [1, 3, 7, 6, 2, 5, 4]

node = TreeNode(4)
```

---

```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```
Terminated, return reversed ans.
```python
stack = []
ans = [1, 3, 7, 6, 2, 5, 4]
```

## Code

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if root:
            stack = [root]
            inv_ans = []

            while stack:
                node = stack.pop()

                if node is not None:
                    inv_ans.append(node.val)

                if node.left is not None:
                    stack.append(node.left)

                if node.right is not None:
                    stack.append(node.right)

            return inv_ans[::-1]
        else:
            return []
```