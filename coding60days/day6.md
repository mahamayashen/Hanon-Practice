# Day 6 Hash Table I
## [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
```python
# Use Dictionary
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        dic = {}
        for char in s:
            if char not in dic:
                dic[char] = 1
            else:
                dic[char] += 1
        
        for char in t:
            if char in dic:
                dic[char] -= 1
            else:
                return False
        
        return all(value == 0 for value in dic.values())
```
---
```python
# Use Array
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0] * 26
        for c in s:
            idx = ord(c) - ord('a')
            record[idx] += 1

        for c in t:
            idx = ord(c) - ord('a')
            record[idx] -= 1
        
        return all(ele == 0 for ele in record)
```
---
```python
# Use Counter
from collections import Counter
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        s_ct = Counter(s)
        t_ct = Counter(t)
        return s_ct == t_ct
```

## [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

## [202. Happy Number](https://leetcode.com/problems/happy-number/)
```python
class Solution:
    def _digit_square_sum(self, num: int) -> int:
        total = 0
        while num:
            num, remainder = divmod(num, 10)
            total += remainder ** 2
        return total

    def isHappy(self, n: int) -> bool:
        seen = set()
        while n != 1 and n not in seen:
            seen.add(n)
            n = self._digit_square_sum(n)
        return n == 1
```

### The `divmod()` Function
**`divmod(a, b)`** is a built-in Python function that **divides `a` by `b` and returns both the quotient and the remainder** as a tuple.

- **Syntax:** `divmod(a, b)`
- **Returns:** A tuple `(quotient, remainder)`
- **Example:**
    - `divmod(123, 10)` returns `(12, 3)`
    - `divmod(12, 5)` returns `(2, 2)`
    - `divmod(1, 10)` returns `(0, 1)`

## [1. Two Sum](https://leetcode.com/problems/two-sum)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # value: index
        for idx, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], idx]
            seen[num] = idx
```
