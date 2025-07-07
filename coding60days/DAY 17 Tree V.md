### [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

This is my first solution. It has **O(n²) time complexity** because, at each step, it scans a subarray to find the maximum value, and the total length of all subarrays scanned across all recursive calls sums to O(n²). This makes it inefficient for large input arrays.

```python
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        def helper(st, ed):
            if st > ed:
                return
            rootVal = max(nums[st:ed+1]) 
            rootIdx = nums.index(rootVal, st, ed+1)
            root = TreeNode(rootVal)
            root.left = helper(st, rootIdx-1)
            root.right = helper(rootIdx+1, ed)
            return root
        
        return helper(0, len(nums) - 1)
```

>[!info] 
>- `sequence.index(x, start, end)`:  This method searches for the first occurrence of `x` within the slice `sequence[start:end]`
>- The time complexity of the index() method in Python, when used with start and end parameters, is **O(k)**, where k is the length of the sub-sequence being searched.

- [ ] Try to use #monotonic_stack to solve this problem.

### [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

```python
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        # return root1
        if not root1 and not root2:
            return None
        elif not root1:
            return root2
        elif not root2:
            return root1
        else:
            root1.val += root2.val
            root1.left = self.mergeTrees(root1.left, root2.left)
            root1.right = self.mergeTrees(root1.right, root2.right)
            return root1
```

| Case                      | What to Return                      | Why?                            |
| ------------------------- | ----------------------------------- | ------------------------------- |
| Both trees are `None`     | `None`                              | Nothing to merge                |
| Only `root1` is `None`    | `root2`                             | Preserve existing nodes         |
| Only `root2` is `None`    | `root1`                             | Preserve existing nodes         |
| Both trees are not `None` | Merged node and recurse on children | Combine and recurse on children |
>[!attention] 
>- when you recursively construct or reconstruct a tree, you should always return a node (or `None` for empty branches) at each recursive step.
>- This pattern applies whether you are building a tree from scratch, merging two trees, or transforming a tree structure.

### [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
        elif val == root.val:
            return root
        elif val < root.val:
            return self.searchBST(root.left, val) 
        elif val > root.val:
            return self.searchBST(root.right, val)
```

| Condition         | What Happens                        | Why Return?              |
| ----------------- | ----------------------------------- | ------------------------ |
| `val == root.val` | Return `root`                       | Found the node           |
| `val < root.val`  | Search left subtree, return result  | Pass result up the stack |
| `val > root.val`  | Search right subtree, return result | Pass result up the stack |
| `root is None`    | Return `None`                       | Not found                |
>[!attention] 
>You need to return the recursive calls so that the result of the search — whether a node or `None` — is correctly passed back through all levels of recursion to the original caller. Without the return, the search result would be lost.

### [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

>[!error] 
>- only checks if `root.val > root.left.val` and `root.val < root.right.val` is not sufficient.
>  - A valid BST requires **all values in the left subtree** to be strictly less than `root.val`, and **all values in the right subtree** to be strictly greater than `root.val`.

```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        
        def dfs(node, low, high):
            if not node:
                return True
            if not (low < node.val < high):
                return False
            return dfs(node.left, low, node.val) and dfs(node.right, node.val, high)
        
        return dfs(root, -float('inf'), float('inf'))
```

Another way is to leverage the property - **in-order traversal of a BST yields a strictly increasing sequence**. This means that as we traverse the tree in in-order (left, root, right), each node’s value should be greater than the previously visited value. By maintaining a variable (such as `self.prev`) to track the last value seen, we can check this property efficiently during traversal.
[The following solution is inspired by programmercarl.](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC) 
```python
class Solution:
    def isValidBST(self, root):
        self.prev = float('-inf')
        def in_order(node):
            if node is None:
                return True
            left = in_order(node.left)
            if node.val <= self.prev:
                return False
            self.prev = node.val
            right = in_order(node.right)
            return left and right
        return in_order(root)
```

If at any point the current node’s value is not greater than `self.prev`, we know the BST property is violated and can return `False` immediately. The following version demonstrates an optimization: by returning `False` as soon as a violation is detected in the left subtree, the function avoids unnecessary checks, making the validation both clear and efficient.

```python
class Solution:
    def isValidBST(self, root):
        self.prev = float('-inf')
        def in_order(node):
            if node is None:
                return True
            if not in_order(node.left):
                return False
            if node.val <= self.prev:
                return False
            self.prev = node.val
            return in_order(node.right)
        return in_order(root)
```