# day5_二维bfs

---

## Leetcode Review

---

[429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

```
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
##-----
##bfs层级遍历搭配deque,注意deque.extend只能用于此种多分叉层级遍历不适用二叉树
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: Node
        :rtype: List[List[int]]
        """
        if not root:
            return []
        q = collections.deque([root])
        res = []
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                q.extend(node.children)
            res.append(level)
            
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

```
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
##-----
##bfs层级遍历搭配deque,注意deque.extend只能用于此种多分叉层级遍历不适用二叉树
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: Node
        :rtype: List[List[int]]
        """
        if not root:
            return []
        q = collections.deque([root])
        res = []
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                q.extend(node.children)
            res.append(level)
            
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
