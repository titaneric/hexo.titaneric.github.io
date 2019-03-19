---
title: Permutation-Sequence
date: 2019-02-26 00:33:42
tags:
 - problem
 - python
---
# [Permutation Sequence (#60)](https://leetcode.com/problems/permutation-sequence/)

## Description

The set [1,2,3,...,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:

1. "123"
2. "132"
3. "213"
4. "231"
5. "312"
6. "321"

Given n and k, return the kth permutation sequence.

---

## Example

Example 1
```
Input: n = 3, k = 3
Output: "213"
```

Example 2
```
Input: n = 4, k = 9
Output: "2314"
```

<!--more-->

---

## [Solution](https://leetcode.com/problems/permutation-sequence/discuss/22507/%22Explain-like-I'm-five%22-Java-Solution-in-O(n))

### Observation

Given n = 4 and k = 5.

Emumerate part of permutations:

1. "1234"
2. "1243"
3. "1324"
4. "1342"
5. "1423"
6. "1432"
7. ...

Should return "1423"

---

All permutations are consist of

```
1 + permutation([2, 3, 4]), size = 3!
2 + permutation([1, 3, 4]), size = 3!
3 + permutation([1, 2, 4]), size = 3!
4 + permutation([1, 2, 3]), size = 3!
```
5 is located at 1st group!
So the first number is **1**.

---

How to determine the group index?
By **division**!

For convenient, convert k to zero base.
E.g., 5 - 1 = 4.

**Q**: There are $n$ groups, and each group is $(n - 1)!$ size, what's the group index of $k$?

**A**: It's $k / (n - 1)!$

E.g.
$4 / (4 - 1)! = 0$

---

<div style="color: #8a6d3b">
After determining the group number, the number is in **ordered candidates** at group index.
</div>

E.g.
```
candidates = [1, 2, 3, 4]
              0  1  2  3

group index = 0
```
Get the first number 1.

---

How to determine rest of numbers?
Let's look at new permutations.

```
2 + permutation([3, 4]), size = 2!
3 + permutation([1, 4]), size = 2!
4 + permutation([2, 3]), size = 2!
```

New k become $4 - 0 \times (4 - 1)! = 4$.
That is also equal to $4  \, \% \, (4 - 1)! = 4$.

<div style="color: #8a6d3b">
- $\text{index} = \text{index} \, \% \, \lvert\text{candidates}\rvert!$
- $\text{candidates} = \text{candidates} \setminus \{\text{selected number}\}$
</div>

---

Continue...
```
k = 4
candidates = [2, 3, 4]
```
Group index = $4 / (3 - 1)! = 2$
```
candidates = [2, 3, 4]
              0  1  2

group index = 2
```

Select 4

new candidates = `[2, 3]`
new k = $4 \% (3 - 1)! = 0$

---

Continue...
```
k = 0
candidates = [2, 3]
```
Group index = $0 / (2 - 1)! = 0$
```
candidates = [2, 3]
              0  1

group index = 0
```

Select 2

new candidates = `[3]`
new k = $0 \% (2 - 1)! = 0$

---

Finally...
```
k = 0
candidates = [3]
```
Group index = $0 / (1 - 1)! = 0$
```
candidates = [3]
              0

group index = 0
```

Select 3

---

### Algorithm

1. Find out which group k is located by division
2. Use the quotient to index the quotient
3. Update candidates, k as remainder
4. When candidates length is not zero, goto 1

Time Complexity: $O(n)$
Space Complexity: $O(n)$

---

### Code

```python
class Solution:
    def getPermutation(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: str
        """
        factorial = [1]
        for i in range(1, n + 1):
            factorial.append(factorial[-1] * i)

        nums = list(range(1, n + 1))

        k -= 1
        s = ["0" for _ in range(n)]

        for i in reversed(range(n)):
            index, k = divmod(k, factorial[i])
            s[n - 1 - i] = str(nums[index])
            nums.pop(index)

        return "".join(s)
```
