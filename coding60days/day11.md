# Day 11 Stack & Queue II

## [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        ops = ['+', '-', '*', '/']
        st = []
        for t in tokens:
            if t in ops:
                num2 = st.pop()
                num1 = st.pop()
                if t == ops[0]:
                    newNum = num1 + num2
                elif t == ops[1]:
                    newNum = num1 - num2
                elif t == ops[2]:
                    newNum = num1 * num2
                elif t == ops[3]:
                    newNum = int(num1 / num2)
                st.append(newNum)
            else:
                st.append(int(t))

        return st.pop()
```

## [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
### Monotonic Queue
[灵茶山艾府解法](https://www.bilibili.com/video/BV1bM411X72E?spm_id_from=333.788.videopod.sections&vd_source=e492103ac776ad055e020b9f09bc74ac)

```python
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        q = deque() # idx
        ans = []

        for i, x in enumerate(nums):
            # 1. in
            while q and nums[q[-1]] <= x:
                q.pop()
            q.append(i)
            # 2. out
            if i - q[0] >= k:
                q.popleft()
            # 3. record
            if i >= k-1:
                ans.append(nums[q[0]])
        return ans
```

### [Introduction to heapq Module from Real Python](https://realpython.com/python-heapq-module/)
## [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
```python
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        records = {}
        for n in nums:
            records[n] = records.get(n, 0) + 1
        
        # Use heapq.nlargest to get the k keys with the highest frequency
        top_k = heapq.nlargest(k, records, key=lambda x: records[x])
        return top_k
```
### Time and Space Complexity

- **Time Complexity:**
    - Building the frequency dictionary: O(n), where n is the length of `nums`.
    - `heapq.nlargest` on the dictionary keys: O(m log k), where m is the number of unique elements (since it maintains a heap of size k).
    - Overall: **O(n + m log k)**, which is efficient for this problem.
- **Space Complexity:**
    - Frequency dictionary: O(m), where m is the number of unique elements.
    - Heap for `nlargest`: O(k).
    - Overall: **O(m + k)**.
