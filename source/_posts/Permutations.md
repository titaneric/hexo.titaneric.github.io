---
title: Permutations
date: 2019-03-12 01:46:30
tags:
 - python
 - problem
---

## [Permutations (#46)](https://leetcode.com/problems/permutations/)

## Description

Given a collection of **distinct** integers, return all possible permutations.

---

## Example

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

---

## Solutions

1. Recursive function (Backtracking).
2. Use **Permutation Sequence**.
3. Use **Next Permutation**.

---

### Recursive (Backtracking)

1. If the incoming `permutation` length is equal to `Input` length, store it.
2. Otherwise, for every element  of `Input`:
   2.1 If element is visted, continue
   2.2 Otherwise, visit it and add to `permutation` and recursive call with updated `permutation`.
   2.3 Pop the last element of `permutation` and un-visit the element.

---

### Use Permutation Sequence.

1. Sort the given `Input`.
2. for i in 1..$n!$:
   2.1 Use the {% post_link Permutation-Sequence %} with index `i` to generate the permutation.
   2.2 Store it.

---

### Use Next Permutation.

1. Sort the given `Input` and put it to `seq`.
2. Store `seq`
3. i = 1
4. while i is not equal to `n!`:
   4.1 Use the {% post_link Next-Permutation %} to generate the next permutation.
   4.2 Store it.

---

### Code (Backtracking)

```python
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # return [list(p) for p in permutations(nums)]

        permute_list = []
        visted = [False for _ in range(len(nums))]
        self.backtracking(permute_list, [], visted, nums)
        return permute_list


    def backtracking(self, permute_list, permutes, visted, nums):
        if len(permutes) == len(nums):
            permute_list.append(permutes.copy())
            return

        for i in range(len(visted)):
            if visted[i]:
                continue
            visted[i] = True
            permutes.append(nums[i])
            self.backtracking(permute_list, permutes, visted, nums)
            permutes.pop()
            visted[i] = False
```
