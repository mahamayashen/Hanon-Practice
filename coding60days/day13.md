# Day 12 Tree II

## [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
[卡哥的视频](https://www.bilibili.com/video/BV1sP4y1f7q7/?vd_source=e492103ac776ad055e020b9f09bc74ac)把递归的方法讲得特别好<br>
[文字版参考](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
```python
# 1.recursive (pre-order / post-order)
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None

        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)

        return root

#2. iterative
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None

        q = deque([root])
        while q:
            node = q.popleft()
            node.left, node.right = node.right, node.left
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        return root
```

## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
[卡哥文字版参考](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html)
```python
# 1.recursive
class Solution:
    def isInvert(self, tree1, tree2) -> bool:
        # Base case: both nodes are None
        if tree1 is None and tree2 is None:
            return True
        # One node exists, the other doesn't
        if tree1 is None or tree2 is None:
            return False
        # Values don't match
        if tree1.val != tree2.val:
            return False
        # Recursively check mirrored subtrees
        return (self.isInvert(tree1.left, tree2.right) and 
                self.isInvert(tree1.right, tree2.left))

    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        # root will not be empty
        return self.isInvert(root.left, root.right)

# 2.iterative
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        q = deque()
        q.append(root.left)
        q.append(root.right)
        while q:
            leftNode = q.popleft()
            rightNode = q.popleft()
            if not leftNode and not rightNode:
                continue
            
            #左右一个节点不为空，或者都不为空但数值不相同，返回false
            if not leftNode or not rightNode or leftNode.val != rightNode.val:
                return False
            q.append(leftNode.left)
            q.append(rightNode.right)
            q.append(leftNode.right)
            q.append(rightNode.left)
        return True
```

## [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
```python
# 1.recursive
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        ans = 0
        def dfs(node: Optional[TreeNode], depth: int) -> None:
            if node is None:
                return
            depth += 1
            nonlocal ans
            ans = max(ans, depth)
            dfs(node.left, depth)
            dfs(node.right, depth)
        dfs(root, 0)
        return ans

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        ans = 0

        def dfs(node, depth):
            if not node:
                return
            depth += 1
            nonlocal ans
            ans = max(ans, depth)
            for child in node.children:
                dfs(child, depth)
        
        dfs(root, 0)
        return ans

# 2.iterative
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        max_depth = 0
        if not root: return 0
        q = deque([root])
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            max_depth += 1
        
        return max_depth
```

## [559. Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/)
```python
# 1.recursive
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        if not root.children:
            return 1
        return 1 + max(self.maxDepth(child) for child in root.children)

# 2.iterative
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root: return 0
        q = deque([root])
        max_depth = 0

        while q:
            for _ in range(len(q)):
                node = q.popleft()
                for child in node.children:
                    q.append(child)
            max_depth += 1

        return max_depth
```

## [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
[灵神的题解](https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/)
```python
# 1.recursive
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # leaf - no kids
        ans = inf
        def f(node, depth):
            nonlocal ans
            if not node:
                return
            # Prune this branch
            if depth >= ans:
                return
            # node is leaf
            if node.left is node.right:
                ans = min(ans, depth)
                return
            f(node.left, depth + 1)
            f(node.right, depth + 1)
        
        f(root, 1)
        
        return ans if root else 0
        
# 2.iterative
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        # use queue
        queue = deque([(root, 1)])
        while queue:
            node, depth = queue.popleft()
            if node.left is node.right:
                return depth
            if node.left:
                queue.append((node.left, depth+1))
            if node.right:
                queue.append((node.right, depth+1))
```
