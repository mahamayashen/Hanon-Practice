### [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

Intuitively, using BFS is easier. Since we want the left bottom most value, appending right nodes before left is a good approach.
```python
class Solution:
	def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
		q = deque([root])
		while q:
			for _ in range(len(q)):
				node = q.popleft()
				if node.right: q.append(node.right)
				if node.left: q.append(node.left)
		return node.val
```

### [112. Path Sum](https://leetcode.com/problems/path-sum/)
Though I just did this question one month ago, it's still hard for me to wrap my head about it for recursion. 
```python
class Solution:
	def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
		if not root: return False
		q = deque([(root, 0)]) # node, so far sum
		while q:
			node, s = q.popleft()
			s += node.val
			if not node.left and not node.right and s == targetSum:
			return True
			if node.left: q.append((node.left, s))
			if node.right: q.append((node.right, s))
		return False
```

Here is [one recursion solution from 灵神](https://leetcode.cn/problems/path-sum/solutions/2731531/jian-ji-xie-fa-pythonjavacgojsrust-by-en-icwe/)
```python
class Solution:
	def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
		if not root:
			return False
		if not root.left and not root.right:
			return root.val == targetSum
		targetSum -= root.val
		return self.hasPathSum(root.left, targetSum) or self.hasPathSum(root.right, targetSum)
```

### [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)
```python
# iterative solution using queue
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root: return []
        q = deque([(root, [root.val])])
        ans = []
        while q:
            node, path = q.popleft()
            if not node.left and not node.right and sum(path) == targetSum:
                ans.append(path)
            if node.left:
                q.append((node.left, path + [node.left.val]))
            if node.right:
                q.append((node.right, path + [node.right.val]))
        
        return ans

# recursive with backtracking
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        ans = []
        def dfs(node, current_sum, path):
            if not node:
                return
            current_sum += node.val
            path.append(node.val)
            if not node.left and not node.right:
                if current_sum == targetSum:
                    ans.append(list(path))
            else:
                dfs(node.left, current_sum, path)
                dfs(node.right, current_sum, path)
            path.pop()  # Backtrack

        dfs(root, 0, [])
        return ans
```

### [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        
        def helper(in_st, in_end, post_st, post_end):
            if in_st > in_end or post_st > post_end:
                return None
            rootVal = postorder[post_end]
            rootIdxInorder = inorder.index(rootVal)
            root = TreeNode(rootVal)
            leftSize = rootIdxInorder - in_st
            root.left = helper(in_st, in_st+leftSize-1, post_st, post_st+leftSize-1)
            root.right = helper(rootIdxInorder+1, in_end, post_st+leftSize, post_end-1)
            return root
        
        n = len(inorder)
        return helper(0, n-1, 0, n-1)
```
This solution currently uses `inorder.index(rootVal)` inside the helper function, which is <mark style="background: #8ED27B96;">**O(n)**</mark> for each recursive call. I will create a hashmap to map each value to its index in the inorder list in the next solution.

### [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        val2idx = {inorder[idx]:idx for idx in range(len(inorder))}
        
        def helper(pre_st, pre_ed, in_st, in_ed):
            if pre_st > pre_ed or in_st > in_ed:
                return
            rootVal = preorder[pre_st]
            rootIdx = val2idx[rootVal]
            leftSize = rootIdx - in_st
            root = TreeNode(rootVal)
            root.left = helper(pre_st+1, pre_st+leftSize, in_st, rootIdx-1)
            root.right = helper(pre_st+leftSize+1, pre_ed, rootIdx+1, in_ed)
            return root

        n = len(inorder)
        return helper(0, n-1, 0, n-1)
```