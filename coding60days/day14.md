# Day 14 Tree III

## [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
[灵神解法](https://www.bilibili.com/video/BV18M411z7bb?vd_source=e492103ac776ad055e020b9f09bc74ac&spm_id_from=333.788.videopod.sections)
```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def get_height(node): # return height of this tree
            if not node:
                return 0
            left_height = get_height(node.left)
            if left_height == -1: return -1
            right_height = get_height(node.right)
            if right_height == -1 or abs(right_height - left_height) > 1: return -1

            return max(left_height, right_height) + 1
        
        return get_height(root) != -1
```

## [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

```python
# 1. recursive without backtracking
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        # has root
        ans = []
        def dfs(node, path = ''):
            path += str(node.val)
            if not node.left and not node.right:
                ans.append(path)
            if node.left:
                dfs(node.left, path + '->')
            if node.right:
                dfs(node.right, path + '->')

        dfs(root, '')
        return ans 

# 2. recursive with backtracking
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        # has root
        ans = []
        def dfs(node, path):
            path.append(str(node.val))
            if not node.left and not node.right:
                ans.append('->'.join(path))
            else:
                if node.left:
                    dfs(node.left, path)
                if node.right:
                    dfs(node.right, path)
            path.pop()  # Backtrack: remove the last node before returning

        dfs(root, [])
        return ans
```

## [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)

```python
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        s = 0

        def dfs(node, left: bool):
            if not node:
                return
            if left and not node.left and not node.right:
                nonlocal s
                s += node.val
            if node.left: dfs(node.left, True)
            if node.right: dfs(node.right, False)
        
        dfs(root, False)
        return s
```

## [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

```python
# 1.recursive
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:

        def dfs(node):
            if not node:
                return 0
            leftCnt = dfs(node.left)
            rightCnt = dfs(node.right)
            return 1 + leftCnt + rightCnt
        
        return dfs(root)

# 2.iterative
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        q = deque([root])
        cnt = 0
        while q:
            node = q.popleft()
            cnt += 1
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        
        return cnt
```