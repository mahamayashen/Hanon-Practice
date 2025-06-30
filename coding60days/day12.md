# Day 12 Tree I
Today it's all about **TRAVERSAL**. See:<br>
[144. Binary Tree Preorder Traversal
](https://leetcode.com/problems/binary-tree-preorder-traversal/)<br>
[94. Binary Tree Inorder Traversal
](https://leetcode.com/problems/binary-tree-inorder-traversal/)<br>
[145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)<br>
[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## DFS Recursion

```python
# 1. pre-order
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if not node:
                return
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        return res

# 2. in-order
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if not node:
                return
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        
        dfs(root)
        return res

# 3. post-order
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if not node:
                return
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)

        dfs(root)
        return res
```

## DFS Iterative I
The three iterative methods are not uniform in style. The in-order traversal, in particular, employs a pointer. Unlike the recursive section—where the three implementations differed only slightly—these iterative solutions use distinct logic and structure.
```python
# 1. pre-order
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        st = [root]
        res = []
        while st:
            node = st.pop()
            res.append(node.val)
            if node.right:
                st.append(node.right)
            if node.left:
                st.append(node.left)
        return res

# 2.in-order
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        st = []
        cur = root
        while cur or st:
            if cur:  # Dive left until dead end
                st.append(cur)
                cur = cur.left  # Prioritize left subtree
            else:
                cur = st.pop()  # Backtrack: retreat to last unprocessed node
                res.append(cur.val)  # Process node after left subtree
                cur = cur.right  # Explore right subtree
        
        return res

# 3. post-order
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        st = [root]
        res = []
        while st:
            node = st.pop()
            res.append(node.val)
            if node.left:
                st.append(node.left)
            if node.right:
                st.append(node.right)
            
        return res[::-1]
```

## Unifying Iterative Tree Traversals - DFS
Is there a way to implement all three traversals—pre-order, in-order, and post-order—using a single, consistent iterative pattern? Meet the **stack-of-pairs** method! This approach (also called iterative traversal with state tracking) uses a simple stack where each entry is a tuple `(node, visited)`. The boolean `visited` flag indicates whether a node has been processed, allowing us to handle all traversal types with nearly identical code.

```python
# 1. pre-order
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        st = [(root, False)] if root else []
        res = []

        while st:
            node, visited = st.pop()
            if visited:
                res.append(node.val)
                continue
            if node.right:
                st.append((node.right, False))
            if node.left:
                st.append((node.left, False))
            st.append((node, True))

        return res

# 1. pre-order
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        st = [root]
        res = []
        while st:
            node = st.pop()
            res.append(node.val)
            if node.right:
                st.append(node.right)
            if node.left:
                st.append(node.left)
        return res

# 2.in-order
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        st = [(root, False)] if root else []
        res = []

        while st:
            node, visited = st.pop()
            if visited:
                res.append(node.val) 
                continue
            if node.right:
                st.append((node.right, False))
            st.append((node, True))
            if node.left:
                st.append((node.left, False))
        
        return res

# 3. post-order
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        st = [(root, False)] if root else []
        res = []

        while st:
            node, visited = st.pop()
            if visited:
                res.append(node.val)
                continue
            st.append((node, True))
            if node.right:
                st.append((node.right, False))
            if node.left:
                st.append((node.left, False))
        
        return res
```

## BFS
[灵神的视频讲解](https://www.bilibili.com/video/BV1hG4y1277i?spm_id_from=333.788.videopod.sections&vd_source=e492103ac776ad055e020b9f09bc74ac)

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        q = deque([root])
        res = []
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            res.append(level)
        
        return res
```

## [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
```python
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        q = deque([root])
        ans = []

        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            if len(ans) % 2 == 0:
                ans.append(level)
            else:
                ans.append(level[::-1])

        return ans
```