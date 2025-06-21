# Day 8 String I
## [344. Reverse String](https://leetcode.com/problems/reverse-string/)
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s[:] = s[::-1]
```
The key difference is that `s[:] = s[::-1]` **modifies the original list in-place** by replacing its contents with the reversed version, so any variable referencing that list outside the function will see the change. In contrast, `s = s[::-1]` creates a new reversed list and assigns it to the local variable s, leaving the original list untouched; this means changes are not reflected outside the function. 

Essentially, **slice assignment (`s[:] = ...`) affects the actual object, while variable assignment (`s = ...`) only changes what the local variable points to.**

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s[:] = reversed(s)
```

## [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/)
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        n = len(s)
        res = ''

        for i in range(0, n, 2*k):
            reversed_chunk = s[i:i+k][::-1]
            next_chunk = s[i+k:i+2*k]
            res += reversed_chunk + next_chunk
        
        return res
```

## [54. 替换数字](https://kamacoder.com/problempage.php?pid=1064)
```python
import sys

def substituteNumbers(s:str) -> str:
    res = ''
    for c in s:
        if c.isdigit():
            res += 'number'
        else:
            res += c
    return res


def main():
    s = sys.stdin.read()
    res = substituteNumbers(s)
    print(res)

if __name__ == "__main__":
    main()
```
`isdigit()`: Returns True only if all characters in the string are digits (0-9). It will return False for negative numbers, decimals, or numbers with exponents.

`isnumeric()`: Similar to isdigit(), but also considers characters with Unicode numeric value features, like fractions or superscripts. It still returns False for negative signs and decimal points.
