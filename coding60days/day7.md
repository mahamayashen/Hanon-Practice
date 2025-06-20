# Day 7 Hash Table II
## [454. 4Sum II](https://leetcode.com/problems/4sum-ii/)
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        n = len(nums1)
        seen = {} # sum: cnt
        for i in range(n):
            for j in range(n):
                sum1 = nums1[i] + nums2[j]
                seen[sum1] = seen.get(sum1, 0) + 1

        cnt = 0
        for i in range(n):
            for j in range(n):
                sum2 = nums3[i] + nums4[j]
                if -sum2 in seen:
                    cnt += seen[-sum2]
        
        return cnt
```
```python
from collections import Counter
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        n = len(nums1)
        firstSum = Counter(a + b for a in nums1 for b in nums2)

        cnt = 0
        for c in nums3:
            for d in nums4:
                cnt += firstSum.get(-(c + d), 0)
        
        return cnt
```

## [383. Ransom Note](https://leetcode.com/problems/ransom-note/)
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        mag = {}
        for char in magazine:
            mag[char] = mag.get(char, 0) + 1
        
        for char in ransomNote:
            if char not in mag or mag[char] == 0:
                return False
            mag[char] -= 1
        
        return all(v >= 0 for v in mag.values())
```
## [15. 3Sum](https://leetcode.com/problems/3sum/)
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        ans = []
        # i, j, k are three pointers
        # fix i; j，k are two opposite pointers
        for i in range(n - 2):
            # Skip duplicate values for fixed i
            if i > 0 and nums[i] == nums[i-1]:
                continue
            # Prune the search space
            # If the largest possible sum is less than 0, skip
            if nums[i] + nums[n-1] + nums[n-2] < 0:
                continue
            # If the smallest possible sum is greater than 0, break
            if nums[i] + nums[i+1] + nums[i+2] > 0:
                break
            j, k = i + 1, n - 1
            target = - nums[i]
            while j < k:
                total = nums[j] + nums[k]
                if total == target:
                    # Found a triplet
                    ans.append([nums[i], nums[j], nums[k]])
                    # Move j and k to the next different elements
                    # Avoid duplicates
                    j += 1
                    while j < k and nums[j] == nums[j-1]:
                        j += 1
                    k -= 1
                    while j < k and nums[k] == nums[k+1]:
                        k -= 1
                elif total < target:
                    j += 1
                elif total > target:
                    k -= 1

        return ans
```

## [18. 4Sum](https://leetcode.com/problems/4sum/)
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        ans = []
        n = len(nums)
        # a, b, c, d
        for a in range(n - 3):
            # skip duplicated value in a
            if a > 0 and nums[a] == nums[a-1]:
                continue
            # Prune 1
            if nums[a] + nums[a+1] + nums[a+2] + nums[a+3] > target:
                break
            # Prune 2
            if nums[a] + nums[n-3] + nums[n-2] + nums[n-1] < target:
                continue
            for b in range(a + 1, n - 2):
                # skip duplicated value in b
                if b > (a + 1) and nums[b] == nums[b-1]:
                    continue
                # Prune 1
                if nums[a] + nums[b] + nums[b+1] + nums[b+2] > target:
                    break
                # Prune 2
                if nums[a] + nums[b] + nums[n-2] + nums[n-1] < target:
                    continue
                c, d = b + 1, n - 1
                while c < d:
                    total = nums[a] + nums[b] + nums[c] + nums[d]
                    if total < target:
                        c += 1
                    elif total > target:
                        d -= 1
                    else:
                        ans.append([nums[a], nums[b], nums[c], nums[d]])
                        c += 1
                        while c < d and nums[c] == nums[c-1]:
                            c += 1
                        d -= 1
                        while c < d and nums[d] == nums[d+1]:
                            d -= 1
        return ans          
```