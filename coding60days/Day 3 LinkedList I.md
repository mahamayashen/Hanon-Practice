## [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode()
        dummy.next = head
        pre = dummy
        while pre.next:
            if pre.next.val == val:
                pre.next = pre.next.next
            else:
                pre = pre.next       
        
        return dummy.next
```

## [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

```python
class DoubleLinkedList:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev


class MyLinkedList:

    def __init__(self, head=None, tail=None, size=0):
        self.head = head
        self.tail = tail
        self.size = size
        
    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        curr = self.head
        for _ in range(index):
            curr = curr.next
        return curr.val

    def addAtHead(self, val: int) -> None:
        newHead = DoubleLinkedList(val)
        if self.head is None:
            self.head = self.tail = newHead
        else:
            newHead.next = self.head
            self.head.prev = newHead
            self.head = newHead
        self.size += 1    

    def addAtTail(self, val: int) -> None:
        newTail = DoubleLinkedList(val)
        if self.tail is None:
            self.head = self.tail = newTail
        else:
            newTail.prev = self.tail
            self.tail.next = newTail
            self.tail = newTail
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        if index == 0:
            self.addAtHead(val)
        elif index == self.size:
            self.addAtTail(val)
        elif 0 < index < self.size:
            curr = self.head
            for _ in range(index-1):
                curr = curr.next
            newNode = DoubleLinkedList(val)
            newNode.prev = curr
            newNode.next = curr.next
            curr.next.prev = newNode
            curr.next = newNode
            self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        curr = self.head
        for _ in range(index):
            curr = curr.next
        if curr.prev:
            curr.prev.next = curr.next
        else:
            self.head = curr.next
        if curr.next:
            curr.next.prev = curr.prev
        else:
            self.tail = curr.prev
        self.size -= 1
```
## [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
![reverse linkedlist](reverse%20linkedlist.gif)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        cur = head
        while cur:
            temp = cur.next     # Save next node
            cur.next = pre      # Reverse the link
            pre = cur           # Move pre forward
            cur = temp          # Move cur forward
        return pre
```
