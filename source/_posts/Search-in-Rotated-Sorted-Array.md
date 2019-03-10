---
title: Search-in-Rotated-Sorted-Array
date: 2019-02-14 01:32:02
tags:
 - problem
 - python
---
# [Search in Rotated Sorted Array (#33)](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Description
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of $O(log\,n)$.

---

## Example

### Example 1

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

### Example 2

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

---

## Solution

### Observation
If we know how far the array *rotated*, we could search the target like normal binary search.

```
nums = [4,5,6,7,0,1,2]
target = 0
```
For example, If we have known the previous array is rotated by 4, then

```
[4,5,6,7,0,1,2]
 0 1 2 3 4 5 6

real target's index is 4

[0,1,2,4,5,6,7]
 0 1 2 3 4 5 6
 
original target's index is 0
```

Is the real target's index equal to original target's index plus rotated num?

However ....
```
nums = [6,7,0,1]
target = 7
```
Again, assert that we have already known the number of rotation is 2.

```
[6,7,0,1]
 0 1 2 3

real target's index is 1

[0,1,6,7]
 0 1 2 3 
 
original target's index is 3
```
1 = 3 + 2

BUT, we could simply fix this by module length of nums, that is
**real target's index == (original target's index + rotated num) % length of nums**
Check our assumption...

- $4 = (0 + 4)\, \% \, 6$
- $1 = (3 + 2)\, \% \, 4$


At last, how could we know the number of rotation?

We just need to find the index of minimum of nums, and we could know the number of rotation is equal to it!

Verify it!

```
[4,5,6,7,0,1,2]
 0 1 2 3 4 5 6
         ^

[6,7,0,1]
 0 1 2 3
     ^
```

How? Please reference below link:
{% post_link Find-Minimum-in-Rotated-Sorted-Array %}

---

### Wrap up

The algorithm are ...

1. Find the index of **minimum** of nums, that is the rotated number.
2. Binary search the index of target in **original array**.
3. return the (index + rotated number) % length of array if index is not equal to -1 else -1

Space complexity: $O(1)$
Time complexity: $O(log \, n)$

---

### Code

```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        shift = self.find_shift(nums)
        # print(shift)
        faked_index = self.shift_binary_search(nums, shift, target)
        return  (faked_index + shift) % len(nums) if faked_index != -1 else -1


    def shift_binary_search(self, nums, shift, target):
        l, h = 0, len(nums) - 1
        while l <= h:
            m = l + (h - l) // 2
            pivot = nums[(m + shift) % len(nums)]
            if pivot == target:
                return m
            elif pivot > target:
                h = m - 1
            else:
                l = m + 1
        return -1

    def find_shift(self, nums):
        l, h = 0, len(nums) - 1
        while l < h:
            m = l + (h - l) // 2
            if nums[m] <= nums[h]:
                h = m
            else:
                l = m + 1
        return l
```
