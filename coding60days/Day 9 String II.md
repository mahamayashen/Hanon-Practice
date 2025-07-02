## [151. Reverse Words in a String]()
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = s.split()[::-1]
        return ' '.join(words)
```

## [55. 右旋字符串](https://leetcode.com/problems/reverse-words-in-a-string/)
```python
import sys

def read_string(data):
    k = int(data[0])
    s = data[1]
    return k, s
    
def rotate_string(k:int, s:str) -> str:
    n = len(s)
    k %= n
    return s[-k:] + s[:-k]

def main():
    data = sys.stdin.read().split()
    k, s = read_string(data)
    print(rotate_string(k, s))

if __name__ == "__main__":
    main()
```
## tbc [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## tbc [459. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)