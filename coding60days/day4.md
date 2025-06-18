# Day 4 LinkedList II
## [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        dummy.next = head
        pre, p1 = dummy, head
        while p1 and p1.next:
            # p1 is the first node of the pair, p2 is the second node
            # temp is the node after the pair
            p2 = p1.next
            temp = p2.next
            # Swap the pair
            # Result: pre -> p2 -> p1 -> temp
            # The sequence of the following three lines can be swapped
            # because we have already stored the next node in temp.
            pre.next = p2
            p2.next = p1
            p1.next = temp
            # Move pointers forward
            pre = p1
            p1 = temp
        
        return dummy.next
```
### Linked List Swap/Reverse Pattern

Most linked list manipulation algorithms (such as swapping pairs or reversing segments) follow a common three-step pattern:

#### 1. **Save Pointers or Initialize References**

**Purpose:**
Preserve access to nodes you’ll need later, preventing loss of the list’s structure.


#### 2. **Perform the Swap, Reverse, or Other Operation**

**Purpose:**
Update the `next` pointers to achieve the desired order.


#### 3. **Move Pointers Forward for the Next Action**

**Purpose:**
Advance the iterator to the next segment of the list.

---

| Step | Description | Example Code |
| :-- | :-- | :-- |
| 1 | Save pointers or initialize references | `temp = p2.next; prev = None` |
| 2 | Perform swap/reverse | `p2.next = p1; p1.next = temp` |
| 3 | Move pointers forward for next action | `prev = p1; p1 = temp` |

---

## [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        l, r = dummy, dummy
        for _ in range(n):
            r = r.next
        while r.next:
            l = l.next
            r = r.next
        l.next = l.next.next
        
        return dummy.next
```

## [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        pA = headA
        pB = headB

        if not pA or not pB:
            return None

        while pA != pB:
            pA = pA.next if pA else headB
            pB = pB.next if pB else headA
        
        return pA
```
### Key Insight
If two linked lists intersect, their tails after the intersection node are identical.

The difference in lengths of the two lists before the intersection can be neutralized by traversing both lists in a way that equalizes the total path length.

## [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
            # If there is a cycle, the slow and fast pointers will eventually meet
            if slow == fast:
                # Move one of the pointers back to the start of the list
                # and move both pointers one step at a time until they meet again
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
                
        # If there is no cycle, return None
        return None
```