#### [Introduction to `deque()` from Real Python](https://realpython.com/python-deque/)
## [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
```python
from collections import deque

class MyQueue:

    def __init__(self):
        self.stack_in = deque()
        self.stack_out = deque()
        
    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        if self.empty():
            return None
        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()
        
    def peek(self) -> int:
        ans = self.pop()
        self.stack_out.append(ans)
        return ans

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)
```
## [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
```python
from collections import deque

class MyStack:
    def __init__(self):
        self.q1 = deque()
        self.q2 = deque()

    def push(self, x: int) -> None:
        # Always push to q2, then move all elements from q1 to q2, then swap
        self.q2.append(x)
        while self.q1:
            self.q2.append(self.q1.popleft())
        # Swap q1 and q2 so q1 always has the current stack
        self.q1, self.q2 = self.q2, self.q1

    def pop(self) -> int:
        return self.q1.popleft()

    def top(self) -> int:
        return self.q1[0]

    def empty(self) -> bool:
        return not self.q1
```

## [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
```python
from collections import deque

class Solution:
    def isValid(self, s: str) -> bool:
        Map = {
            ')':'(',
            '}':'{',
            ']':'['
        }

        stack = deque()

        for c in s:
            if stack and c in Map and stack[-1] == Map[c]:
                stack.pop()
            else:
                stack.append(c)
        
        return len(stack) == 0
```
## [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)
```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for c in s:
            if stack and stack[-1] == c:
                stack.pop()
            else:
                stack.append(c)
        
        return ''.join(stack)
```