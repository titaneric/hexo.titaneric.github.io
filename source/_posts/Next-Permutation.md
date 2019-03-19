---
title: Next-Permutation
date: 2019-02-22 19:24:20
tags:
 - problem
 - python
---

# [Next Permutation (#31)](https://leetcode.com/problems/next-permutation/)

## Description

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

---

## Example

`1, 2, 3` $\rightarrow$ `1, 3, 2`
`3, 2 ,1` $\rightarrow$ `1, 2, 3`
`1, 1, 5` $\rightarrow$ `1, 5, 1`

<!--more-->

---

## Solution

### Observation

What's the pattern of these examples?
```
1, 2, 3  -> 1, 3, 2
      ^        ^
```

```
1, 1, 5  -> 1, 5, 1
      ^        ^
```

```
4, 1, 3, 2 -> 4, 2, 1, 3
      ^          ^
```

```
2, 4, 1, 3 -> 2, 4, 3, 1
         ^          ^
```

```
3, 4, 2, 1 -> 4, 1, 2, 3
   ^          ^
```

<div style="color: #8a6d3b">
It looks like the pointer is the index that is ready to "carry" to next index.
</div>

---

However, how do we find the index that is carrying?
```
1, 2, 3
      <-
```
2 < <span style="color: #D38B5D">3</span>

```
1, 1, 5
      <-
```
1 < <span style="color: #D38B5D">5</span>

```
4, 1, 3, 2
      <---
```
1 < <span style="color: #D38B5D">3</span> > 2

---

More examples...
```
2, 4, 1, 3
         <-
```
1 < <span style="color: #D38B5D">3</span>

```
3, 4, 2, 1
   <------
```
3 < <span style="color: #D38B5D">4</span> > 2 > 1

<div style="color: #8a6d3b">
Find the index that is the peak of ascending order of array from reversed sequence.
</div>

---

After find the index, how should we do carry?

- The replaced carry is **more** than its original.
- The numbers before  carry is **as small as possible**.
- The numbers after carry is **fixed**.

In this time, we need the intermediate...
```
1, 2, 4, 3 -> 1, 3, 4, 2 -> 1, 3, 2, 4
      ^
```
```
3, 4, 2, 1 -> 4, 3, 2, 1 -> 4, 1, 2, 3
   ^ 
```

---

We observe the original and intermediate first
```
1, 2, 4, 3 -> 1, 3, 4, 2
      ^
   o     x
```
```
1, 3, 4, 2 -> 1, 4, 3, 2
      ^
   o  x
```
<div style="color: #8a6d3b">
    <ul>
        <li> From the point of view from carry, the sequence is descending order after the carry.</li>
        <li> Find the number such that its value is <b>slightly larger</b> than carry is the <b>replacement</b>.</li>
    </ul>
</div>

---

Confirm above...


```
1, 4, 3, 2 -> 2, 4, 3, 1
   ^
o        x
```

```
3, 4, 2, 1 -> 4, 3, 2, 1
   ^
o  x
```

<div style="color: #8a6d3b">
After swapping, the sequence is guaranteed to have <b>descending order</b> from the viewpoint from carry. <b>Why?</b>
</div>

One more thing to do...

We now look up the intermediate and the final
```
1, 3, 4, 2 -> 1, 3, 2, 4
      <-->
```

```
4, 3, 2, 1 -> 4, 1, 2, 3
   <----->
```

 We want the numbers after the carry is **as small as possible**, and we have known the sequence is **descending order** after swapping...

Just to **reverse** the numbers after the carry!

---

### Algorithm

1. Find the index that is ready to carry.
2. Find the replaced number that is slightly larger than carry, and swap them.
3. Reverse the sequence after the carry.

Time complexity: $O(n)$

Space complexity: $O(1)$

---

### Code

```python
class Solution:
    def nextPermutation(self, nums: 'List[int]') -> 'None':
        """
        Do not return anything, modify nums in-place instead.
        """
        k = None
        for i in range(len(nums) - 1, 0, -1):
            if nums[i] > nums[i - 1]:
                k = i
                break
        else:
            k = 0

        if k:
            swap = k - 1
            pre = nums[swap]

            pivot = k
            for i in range(k + 1, len(nums)):
                if nums[i] <= pre:
                    break
                else:
                    pivot = i

            nums[swap], nums[pivot] = nums[pivot], nums[swap]

        nums[k:] = nums[k:][::-1]
```